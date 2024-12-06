---
title: "Introduction to Kubernetes"
seoTitle: "Introduction to Kubernetes"
datePublished: Wed Jun 22 2022 07:42:09 GMT+0000 (Coordinated Universal Time)
cuid: cl4wfmly00jk78unvco6o61gg
slug: introduction-to-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1656315514190/aZse392qg.png
tags: kubernetes, k8s, k9, komodor

---

Kubernetes became a central part of the container revolution by making containerized systems significantly more straightforward to handle at scale. Since it's initial release in June 2014, the Kubernetes as a term, has been associated with words like k8s, k9s and very recently with k3s as well.

But before we get started and deep dive into more about - k9s & k3s, let's first get us familiarize ourselves with Kubernetes, shall we?

So, the first thing that comes in mind is ~ *What is kubernetes?*

# What is Kubernetes?

Kubernetes is a standard container orchestration open-source platform, that is, for managing applications constructed from several, often self-contained runtimes called containers. It is a widely used tool to deploy and update the application without any downtime.

It is generally known as k8s or “kube”, where 8 in 'k8s' is a abbreviation for "ubernete".  
Kubernetes also handles things like upgrades of your containers, so when you make a new release of your website, it will gradually launch containers with the new version and gradually kill off the old ones, usually over a minute or two.

## How it helps us?

Kubernetes orchestration allows you to build application services that span multiple containers, schedule those containers across a cluster, scale those containers, and manage the health of those containers over time. With Kubernetes you can take effective steps toward better IT security.

Kubernetes also needs to integrate with networking, storage, security, telemetry, and other services to provide a comprehensive container infrastructure.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656312811622/kldSdyrjG.png align="left")

* Microservices in containers make it\*\* easier to orchestrate services\*\*, including storage, networking, and security. This significantly multiplies the number of containers in your environment, and as those containers accumulate, the complexity also grows.
    
* It fixes a lot of common problems with container proliferation by sorting containers together into "pods." Pods **add a layer of abstraction** to grouped containers.
    
* Other parts of Kubernetes help you\*\* balance loads\*\* across these pods and ensure you have the right number of containers running to support your workloads.
    

## What is Kubernetes-Cluster?

A working Kubernetes deployment is called a cluster. You can visualize a Kubernetes cluster as two parts: **Master and Node**.

* **Node**: Where we run our applications
    
* **Master**: Used to coordinate the cluster.
    

Each node is its own Linux environment, and which could be either a physical or virtual machine and each node runs pods, which are made up of containers.

Master is responsible for managing the cluster. The master nodes will coordinate all the activities in your cluster, such as scheduling applications, maintaining their desired state, scaling applications, and rolling new updates.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656313435775/s5y0nfdyo.png align="left")

# Starting with K9s

**k9s** is a terminal-based UI for managing the Kubernetes clusters, that aims to simplify navigating, observing, and managing applications in k8s. K9s continuously monitors Kubernetes clusters for changes and provides shortcut commands to interact with the observed resources.

## Installation

K9s is open-source software and available for various operating systems with instructions on this official repository. If you use a package manager, K9s will be available through it.

* On Mac OS, you can install K9s with brew: brew install k9s
    
* On Windows, either via *scoop* or *chocolatey*: scoop install k9s choco install k9s
    

### Usage

K9s runs in your terminal, and if you have installed it according to the instructions for your operating system, you should be able to start it with the `k9s` command. It will by default read your current KUBECONFIG, which in turn is by default read from `$HOME/.kube/config.`

Unfortunately, K9s slows down considerably when processing large log amounts. At such moments, K9s wholly used two cores of my Intel Xeon E312xx CPU and could even freeze.

# Issues and Troubleshooting

One of the biggest issue is that, deployment failure messages on Kubernetes are complex and not intuitive for debugging. Users need to identify the errors from pod logs, pod events(describe pod), pod status, etc. Developers spend a lot of time and effort demystifying the cryptic messages K8s throws up, perfectly valuable time that could have otherwise gone in building core functionalities of the application.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656314971586/nRKD_JTv5.png align="left")

## Komodor - A Trouble Shooting Platform

Here **Komodor** makes things interesting by handling the k8s-native troubleshooting , so that developers can save a lot of time actually developing instead of wasting hours of their valuable time troubleshooting and focus on building interesting stuff. It tracks changes across your entire k8s stack, identifies issues, uncovers their root cause and delivers the context you need to troubleshoot efficiently and independently.

Komodor supports configurations per Monitor, meaning it's monitors are being configured on a cluster level, each monitor supports the following configurations:

* **Trigger conditions** - specify when this monitor should be triggered, conditions vary per monitor.
    
* **Scope** - which resources should be monitored, the scope can be configured for the entire cluster, specific namespaces, annotations or labels, the relationship between the selected resources is currently OR relationship.
    
* **Sink/Notification** - where do you want to receive notifications.
    

---

# Follow up Resources

* Kubernetes Basics - [https://kubernetes.io/docs/tutorials/kubernetes-basics/](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
    
* K9s Commands - [https://k9scli.io/topics/commands/](https://k9scli.io/topics/commands/)
    
* Kubernetes Tutorial for Beginners - [https://spacelift.io/blog/kubernetes-tutorial](https://spacelift.io/blog/kubernetes-tutorial)
    
* Komodor Resource Library - [https://komodor.com/resource-library/](https://komodor.com/resource-library/)