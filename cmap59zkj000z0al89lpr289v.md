---
title: "How to Effectively Vet Your Supply Chain for Optimal Performance"
seoTitle: "How to Effectively Vet Your Supply Chain for Optimal Performance"
seoDescription: "Protect your software supply chain using SafeDep's vet tool. Learn to install and integrate for secure, compliant open source libraries"
datePublished: Thu May 15 2025 09:05:51 GMT+0000 (Coordinated Universal Time)
cuid: cmap59zkj000z0al89lpr289v
slug: how-to-effectively-vet-your-supply-chain-for-optimal-performance
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1747299931564/ae5e5f4d-4f45-4755-b069-9bc3fe7898f1.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1747299921032/ae2edf66-a54e-4768-8bc4-f5818d908062.png
tags: opensource, supply-chain-management, open-source, openssf, scanning

---

In today’s world, software projects often rely on many open source libraries. While these libraries speed up development, they can also bring hidden risks if they are not checked carefully. A single unsafe library can compromise an entire project. SafeDep’s **vet** tool helps you guard your software supply chain by checking every library you use. Below is a detailed, step-by-step guide to understanding, installing, and using **vet** in your projects. But ***why is Supply Chain Security matter?***

## Understanding the Risk of Supply Chain Attacks

A **supply chain attack** happens when an attacker hides bad code in a library or package that many developers download. When you add that library to your project, the bad code can:

* Steal sensitive information
    
* Break parts of your application
    
* Spread malware to users
    

### Beyond Traditional Scanning

Supply chain attacks have surged, leveraging tactics such as malicious code injection, dependency confusion, and typosquatting. Conventional scanners focus narrowly on known `CVEs`, leaving blind spots in popularity, maintenance status, license compliance, and more. Modern organizations require a holistic solution that codifies and automates risk evaluation across multiple dimensions.

## Introducing vet: Policy-Driven Supply Chain Protection

**vet** transforms security requirements into executable policies using the Common Expression Language (CEL). By treating security guardrails as code, you achieve:

* **Automated Compliance:** Define rules once and enforce them across every build, pull request, and release. 
    
* **Customizable Risk Tolerance:** Craft filters for critical vulnerabilities, unacceptable licenses, low adoption, or missing maintenance.
    
* **Extensible Metadata:** Leverage OSV vulnerability feeds, popularity metrics, maintenance indicators, extended license attributes, and OpenSSF Scorecards for third-party risk assessment.
    

### Key Features

| **Capability** | **Description** |
| --- | --- |
| **Code Analysis & Filtering** | Focus on high-impact risks using CEL filters to target critical issues only. |
| **Direct OSV Integration** | Pull up-to-date vulnerability data from the OSV ecosystem. |
| **Popularity & Maintenance Checks** | Block unvetted or unmaintained packages before they enter your codebase. |
| **License & Compliance Controls** | Enforce acceptable license policies automatically. |
| **OpenSSF Scorecard Insights** | Incorporate third-party security posture into approval workflows. |
| **Transitive Dependency Coverage** | Analyze both direct and transitive components for end-to-end visibility. |
| **Filter Suites** | Group multiple CEL filters into a single policy suite for complex guardrails. |

## Installation and Setup

Getting started with **vet** is straightforward:

1. **Local CLI Installation**
    
    ```yaml
    brew tap safedep/tap
    brew install safedep/tap/vet
    ```
    
2. **Initial Configuration**  
    Create a `policy.yml` defining your filter suites:
    
    ```yaml
    filter_suites:
      default:
        filters:
          - block_high_severity
          - require_popularity
          - enforce_scorecard
    ```
    
3. **Repository Scan**
    
    ```bash
    vet scan -D . --filter-suite default --fail-on-filter
    ```
    

## Integrating vet into CI/CD Workflows

Seamless CI/CD integration is critical for “shift-left” security. **vet** offers native GitHub Action support, enabling policy execution on every pull request, commit, and release:

```yaml
name: Supply Chain Security

on:
  pull_request:
    paths:
      - '**/*.go'
      - '**/*.js'

jobs:
  vet_scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: safedep/vet-action@v1
        with:
          filter-suite: default
          fail-on-filter: true
```

Upon policy violation, **vet** automatically annotates pull requests with inline comments, detailing the failed filters and suggesting remediation steps. For teams using GitLab or Jenkins, **vet** can be executed via CLI in pipeline stages, with exit codes controlling build success. Furthermore, **vet** can emit SARIF output, integrating with security dashboards and code scanning interfaces for unified visibility.

## Real-World Case Studies

**Scenario:** A global bank with hundreds of Java microservices sought to harden its supply chain against high-severity vulnerabilities and unmaintained packages.

* **Baseline Vulnerability Exposure:** Prior to **vet**, the bank’s DevSecOps team found that, on average, **52%** of pulled dependencies contained at least one high- or critical-severity CVE.
    
* **Policy Implementation:**
    
    * **Block High-Severity CVEs**: `dependency.osv.vulnerabilities.any(v | v.severity in ["HIGH","CRITICAL"])`
        
    * **Maintenance Check**: Reject libraries with zero commits or releases in the last 12 months.
        
* **Integration:** Embedded **vet** into the GitHub Actions pipeline across 120 repositories, running scans on every pull request.
    
* **Results Over Three Months:**
    
    * **85% reduction** in dependencies with high-severity CVEs
        
    * **70% fewer** unmaintained packages entering the codebase, compared to a 30% industry average for mature banking organizations generating SBOMs and enforcing security policies 
        
    * **Mean Time to Remediation (MTTR)** for critical vulnerabilities improved from **72 hours** pre-**vet** to **18 hours**, outpacing the 66% of organizations that remediate within a day
        

**Key Takeaways:** By codifying high-severity and maintenance policies in CEL and automating enforcement, the bank not only slashed vulnerable dependencies but also accelerated remediation workflows, aligning with top-quartile performance in financial services.

## Conclusion

SafeDep’s **vet** redefines software supply chain security through a policy-as-code approach that integrates seamlessly into development workflows. By consolidating metadata from OSV, OpenSSF Scorecards, popularity and maintenance indicators, and license attributes, **vet** provides a comprehensive, real-time defense against diverse supply chain threats.

Organizations can codify their security and compliance requirements in CEL filters, enforce them automatically in CI/CD pipelines, and empower developers with immediate, actionable feedback. Embracing **vet** enables teams to balance innovation and risk, ensuring only secure, compliant open source components advance through their pipelines - ultimately fostering resilient, trustworthy software delivery.

## **FAQ’s**

### **What policies can I define with vet?**

You can express any security or compliance requirement as CEL filters—critical CVEs, banned licenses, low popularity thresholds, end-of-life projects, or OpenSSF Scorecard minima—and group them into reusable filter suites.

### **How does vet obtain vulnerability data?**

vet integrates directly with the OSV ecosystem, pulling the latest vulnerability feeds and mapping them to your dependencies in real time.

### **Can I scan transitive dependencies?**

Yes. vet analyzes both direct and transitive dependencies to ensure end-to-end supply chain visibility and enforcement.

### **How do I integrate vet into my CI/CD pipeline?**

Use the `safedep/vet-action` for GitHub Actions (or analogous steps for other systems) to run `vet scan` on every pull request and build, automatically blocking policy violations before code merges.

### **Is vet extensible to custom metadata sources?**

While vet natively supports OSV, popularity, maintenance, license, and OpenSSF Scorecard data, the CEL-based architecture allows you to incorporate additional metadata feeds or internal risk indicators as needed.