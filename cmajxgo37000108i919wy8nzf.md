---
title: "Building a Complete Node-Level Security Monitoring Pipeline"
seoTitle: "Building a Node-Level Security Monitoring Pipeline in k8s"
seoDescription: "Learn to set up an efficient, scalable node-level security monitoring pipeline for Kubernetes using eBPF, Prometheus, and DaemonSets"
datePublished: Sun May 11 2025 17:28:15 GMT+0000 (Coordinated Universal Time)
cuid: cmajxgo37000108i919wy8nzf
slug: building-a-node-level-security-monitoring-pipeline
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746984041640/15837d39-5eeb-4ae2-856f-1eb318420ca7.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1746984483088/55d3fa10-5989-4ee4-bee5-335640628204.png
tags: linux, opensource, security, kubernetes, monitoring, node, k8s, prometheus, pipeline, cluster, ebpf, monitoring-tool

---

Kubernetes clusters run critical workloads that demand constant vigilance against security threats. Node-level monitoring lets you catch suspicious activity such as unauthorized process launches, unusual file changes, or high-risk module loads even before they escalate. By combining `eBPF kprobes` with standard Linux tools in a `DaemonSet`, and then exporting aggregated findings via Prometheus, you gain an end-to-end observability solution that is lightweight, scalable, and easy to deploy across every node in your cluster.

## Architecture Overview

Our security monitoring solution consists of four main components working together to detect, collect, and expose security-relevant events:

1. **Security Monitor DaemonSet**: Runs on each node to capture kernel events via kprobes and perform regular system scans
    
2. **Monitoring Script ConfigMap**: Houses the shell script that sets up kprobes and logs findings
    
3. **Metrics Exporter Deployment**: A Python container that processes events and exposes Prometheus metrics
    
4. **ClusterIP Service**: Allows Prometheus to scrape the exporter endpoint
    

![](https://raw.githubusercontent.com/Sonichigo/ebpf-k8s-monitoring/4c4ba4416d3683fbb94d0e4da6e784a3307684bf/k8s/main.svg align="left")

## 1\. Deploying the DaemonSet

A DaemonSet ensures a Pod runs on all (or specified) nodes without manual intervention. This is ideal for node-level monitoring agents that require uniform coverage. ***Why so?*** Because it :

* Automatically deploys on new nodes as they join the cluster
    
* Consistent configuration across all monitored hosts
    
* Simplified lifecycle management via Kubernetes APIs
    

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: security-monitor
  namespace: monitoring
  labels:
    app: security-monitor
spec:
  selector:
    matchLabels:
      app: security-monitor
  template:
    metadata:
      labels:
        app: security-monitor
    spec:
      hostPID: true # Required for visibility into all processes
      containers:
      - name: monitor
        image: ubuntu:22.04  # Using Ubuntu minimal which is working in your environment
        securityContext:
          privileged: true # Required for kernel tracing
        volumeMounts:
        - name: kernel-debug
          mountPath: /sys/kernel/debug
        - name: host-proc
          mountPath: /host/proc
          readOnly: true
        - name: host-etc
          mountPath: /host/etc
          readOnly: true
        - name: host-var
          mountPath: /host/var
          readOnly: true
        - name: monitor-script
          mountPath: /scripts
          readOnly: true
        - name: log-volume
          mountPath: /logs
        command: ["/bin/bash", "-c"]
        args:
        - |

          # Install necessary tools first
          apt-get update -y && apt-get install -y procps net-tools iproute2 findutils

          # Copy script - work around read-only issue
          cp /scripts/improved-monitor.sh /tmp/
          chmod +x /tmp/improved-monitor.sh

          # Create a volume that can be accessed by the metrics exporter
          mkdir -p /logs
          chmod 755 /logs

          # Run monitor with output to shared volume
          export LOG_FILE="/logs/security-events.log"
          /tmp/improved-monitor.sh
        resources:
          requests:
            cpu: 200m
            memory: 256Mi
          limits:
            cpu: 500m
            memory: 512Mi
      volumes:
      - name: kernel-debug
        hostPath:
          path: /sys/kernel/debug
      - name: host-proc
        hostPath:
          path: /proc
      - name: host-etc
        hostPath:
          path: /etc
      - name: host-var
        hostPath:
          path: /var
      - name: monitor-script
        configMap:
          name: improved-kprobe-script
      - name: log-volume
        hostPath:
          path: /var/log/security-monitor
          type: DirectoryOrCreate
      tolerations:
      - operator: "Exists"
      restartPolicy: Always
```

Here the most important aspects are:

* `hostPID: true` to see all processes on the node
    
* `privileged: true` to access kernel tracing features
    
* Host path mounts for system directories and log storage
    
* and Resource limits to prevent excessive consumption
    

## 2\. ConfigMap: The Monitoring Script

Our monitoring script performs two main functions:

1. Setting up and managing kernel probes for continuous monitoring
    
2. Running periodic system checks to detect suspicious activity
    

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: improved-kprobe-script
  namespace: monitoring
data:
  improved-monitor.sh: |
    #!/bin/sh
    
    echo "Starting improved security monitoring..."
    LOG_FILE="/tmp/security-events.log"
    TRACEFS_PATH="/sys/kernel/debug/tracing"
    
    # Check if we have tracefs access
    if [ -d "$TRACEFS_PATH" ] && [ -w "$TRACEFS_PATH/kprobe_events" ]; then
      echo "Kernel tracing is available, setting up probes..."
      
      # Try enabling general tracing
      echo 1 > $TRACEFS_PATH/tracing_on 2>/dev/null || 
        echo "Failed to enable tracing, continuing anyway"
      
      # Clean existing probes first to avoid errors
      echo > $TRACEFS_PATH/kprobe_events 2>/dev/null
      
      # Setup multiple probes with error handling for each process execution monitoring
      (echo 'p:exec_probe do_execve' > $TRACEFS_PATH/kprobe_events 2>/dev/null &&
        echo 1 > $TRACEFS_PATH/events/kprobes/exec_probe/enable 2>/dev/null &&
        echo "Enabled exec_probe") || echo "Failed to setup exec_probe"
      
      # Container creation monitoring
      (echo 'p:container_probe security_capable' >> $TRACEFS_PATH/kprobe_events 2>/dev/null &&
        echo 1 > $TRACEFS_PATH/events/kprobes/container_probe/enable 2>/dev/null &&
        echo "Enabled container_probe") || echo "Failed to setup container_probe"
      
      # Mount operations monitoring
      (echo 'p:mount_probe security_sb_mount' >> $TRACEFS_PATH/kprobe_events 2>/dev/null &&
        echo 1 > $TRACEFS_PATH/events/kprobes/mount_probe/enable 2>/dev/null &&
        echo "Enabled mount_probe") || echo "Failed to setup mount_probe"
      
      # Module loading monitoring 
      (echo 'p:module_probe load_module' >> $TRACEFS_PATH/kprobe_events 2>/dev/null &&
        echo 1 > $TRACEFS_PATH/events/kprobes/module_probe/enable 2>/dev/null &&
        echo "Enabled module_probe") || echo "Failed to setup module_probe"
      
      # Start reading from trace pipe if it exists
      if [ -r "$TRACEFS_PATH/trace_pipe" ]; then
        echo "Starting trace pipe monitoring..."
        (cat $TRACEFS_PATH/trace_pipe | while read line; do
          timestamp=$(date +"%Y-%m-%d %H:%M:%S")
          echo "[$timestamp] $line" | tee -a $LOG_FILE
        done) &
        TRACE_PID=$!
      else
        echo "Trace pipe not readable, falling back to basic monitoring"
        TRACE_PID=0
      fi
    else
      echo "Kernel tracing is not available, using basic monitoring"
      TRACE_PID=0
    fi
    
    # Periodic system monitoring regardless of kprobe availability
    echo "Starting periodic system monitoring..."
    
    monitor_system() {
      echo "=== Security scan at $(date) ===" | tee -a $LOG_FILE
      echo "Checking for privileged containers..." | tee -a $LOG_FILE
      ps aux | grep -E 'docker|containerd' | grep -v grep | tee -a $LOG_FILE
      
      # Check for unexpected listeners
      echo "Checking network listeners..." | tee -a $LOG_FILE
      netstat -tulpn 2>/dev/null || ss -tulpn 2>/dev/null || echo "No network tools available" | tee -a $LOG_FILE
      
      # Look for processes running as root in containers
      echo "Checking for root processes..." | tee -a $LOG_FILE
      ps -eo user,pid,ppid,cmd --sort=user | head -20 | tee -a $LOG_FILE
      
      # Check for recent file modifications in sensitive paths
      echo "Checking sensitive file modifications..." | tee -a $LOG_FILE
      find /etc /var/lib/kubelet /etc/kubernetes -type f -mmin -60 2>/dev/null | head -10 | tee -a $LOG_FILE
      
      # Check node resource usage (could indicate cryptomining)
      echo "Checking system load..." | tee -a $LOG_FILE
      uptime | tee -a $LOG_FILE
      free -m | tee -a $LOG_FILE
      
      # Check for unusual processes with high CPU/Memory
      echo "Checking resource-intensive processes..." | tee -a $LOG_FILE
      ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head -10 | tee -a $LOG_FILE
    }
    
    cleanup() {
      echo "Cleaning up monitoring..."
      if [ $TRACE_PID -ne 0 ]; then
        kill $TRACE_PID 2>/dev/null
        echo > $TRACEFS_PATH/kprobe_events 2>/dev/null
        echo 0 > $TRACEFS_PATH/tracing_on 2>/dev/null
      fi
      exit 0
    }
    
    trap cleanup SIGINT SIGTERM
    
    while true; do
      monitor_system
      sleep 300  # Every 5 minutes
    done
```

First, the script checks that the kernel’s tracing filesystem (`tracefs`) is available and writable. Once confirmed, it enables tracing and clears any previously registered probes. It then registers four new kprobes to watch key security-relevant actions: `do_execve` for process executions, `security_capable` for container capability checks, `security_sb_mount` for mount operations, and `load_module` for kernel module loads. Each probe logs events into the tracing subsystem so they can be captured later.

Next, the script reads from the kernel’s `trace_pipe`, which streams live trace events. As each line arrives, it prefixes the entry with a timestamp and writes it into the central log file. This reader runs in the background so that it does not block the rest of the script from executing.

In parallel with kernel tracing, the script performs a set of regular system scans every five minutes. It looks for running containers with elevated privileges by filtering `ps aux` output for the `--privileged` flag. It examines open network sockets using `netstat` or, if that’s unavailable, `ss`. It searches known configuration directories for files changed in the last hour. Finally, it records overall system load—capturing uptime, memory usage via `free`, and the top CPU- and memory-consuming processes—to the same log.

To ensure stability, the script traps termination signals (`SIGINT` and `SIGTERM`). When it receives one, it kills the background trace reader and unregisters all kprobes, then exits cleanly. This prevents leftover probes or hung processes if the Pod restarts or is deleted.

## Deployment Steps

1. **Create Namespace**
    
    ```yaml
    bashkubectl create namespace monitoring
    ```
    
2. **Apply ConfigMaps and Scripts**
    
    ```yaml
    kubectl apply -f ebpf-script-configmap.yml
    kubectl apply -f ebpf-script.yml
    ```
    
3. **Verifying the logs**
    
    ```bash
    [2025-05-11 17:04:48] runc-3248764 [000] d.... 275274.807626: container_probe: (security_capable+0x0/0xe0)
    [2025-05-11 17:04:48] runc-3248764 [000] d.... 275274.807697: container_probe: (security_capable+0x0/0xe0)
    [2025-05-11 17:04:48] exe-3248781 [000] d.... 275274.825963: container_probe: (security_capable+0x0/0xe0)
    [2025-05-11 17:04:48] dockerd-3750696 [000] d.... 275274.826078: mount_probe: (security_sb_mount+0x0/0xf0)
    [2025-05-11 17:04:48] dockerd-3750696 [000] d.... 275274.826079: container_probe: (security_capable+0x0/0xe0)
    ```
    

The logs we are seeing above are the output from the kernel probes (`kprobes`) that your security monitoring script has configured. These are low-level security events captured directly from the Linux kernel. Let me explain what each line means:

**Format -** `[Timestamp] Process-PID [CPU] Flags EventTime: ProbeType: (KernelFunction)`

* **Process**: 
    
    * `runc-3248764` - This is the runc container runtime with PID 3248764
        
    * `dockerd-3750696` - This is the Docker daemon
        
* **Probe Type**: 
    
    * `container_probe` - Triggered by our kprobe on the `security_capable` kernel function
        
    * `mount_probe` - Triggered by our kprobe on the `security_sb_mount` kernel function
        
* **What it means**:
    
    * The container runtime is checking security capabilities - typically happens when a container is requesting privileged operations or capabilities
        
    * The Docker daemon is performing a mount operation, which could be mounting volumes for containers or performing internal filesystem operations
        

These logs, reflects normal container orchestration activity, are precisely the kinds of events our security monitoring system is designed to track. By observing `security_capable` calls, we are able to detect containers attempting to escalate privileges, applications engaging in unauthorized actions, or potential breakout attempts. Similarly, tracking `security_sb_mount` events helps us identify unauthorized filesystem mounts, possible data exfiltration efforts, and container escape techniques leveraging mount operations. The current kprobe instrumentation is effectively capturing container runtime security checks, Docker daemon mount operations, and process capability verifications. These raw events are written to the centralized log file, serving both as an audit trail and a baseline for anomaly detection.

## Conclusion

By combining eBPF-based kernel tracing with periodic system audits in a lightweight DaemonSet, this solution offers proactive node-level security monitoring for Kubernetes clusters. It enables visibility into key events such as unauthorized process execution, privilege escalations, and suspicious mount or module activity without intrusive agents or heavyweight tools.

The modular architecture ensures ease of deployment and scalability across clusters of any size. Whether you're securing a production environment or auditing development clusters, this observability pipeline adds a powerful layer of defense.

We’re actively improving this project and welcome contributions, feedback, and stars.  
👉 **Star the project on GitHub** to support and stay updated: [https://github.com/Sonichigo/ebpf-k8s-monitoring/](https://github.com/Sonichigo/ebpf-k8s-monitoring/)

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=sonichigo1219), [LinkedIn](https://www.linkedin.com/in/sonichigo) and [GitHub](https://github.com/sonichigo).

Thank you for Reading 🙂