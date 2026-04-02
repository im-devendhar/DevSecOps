
---

# DevSecOps Interview Questions & Answers

This repository contains structured and interview-ready notes covering DevSecOps concepts, tools, CI/CD security integration, and real-world scenarios.

---

# 1. Basics

## What is DevOps?

DevOps is a culture and set of practices that integrates development and operations teams to enable faster and reliable software delivery through automation and CI/CD pipelines.

---

## What is DevSecOps?

DevSecOps extends DevOps by integrating security into every stage of the Software Development Life Cycle (SDLC).

* Security is a shared responsibility
* Security is automated in CI/CD pipelines
* Focus on early detection of vulnerabilities

---

## Difference between DevOps and DevSecOps

| DevOps                      | DevSecOps                   |
| --------------------------- | --------------------------- |
| Focus on speed and delivery | Focus on speed and security |
| Security handled later      | Security integrated early   |
| Separate security team      | Shared responsibility       |
| Reactive approach           | Proactive approach          |

---

## What is Shift-Left Security?

Shift-left security means integrating security early in the development lifecycle instead of testing at the end.

Example:

* SAST during coding
* SCA during build

---

# 2. Security Testing Types

## What is SAST?

Static Application Security Testing scans source code without executing it.

* Finds issues like SQL Injection, XSS, hardcoded secrets
* Runs during coding or build stage

---

## What is DAST?

Dynamic Application Security Testing scans a running application.

* Finds runtime vulnerabilities
* Simulates real-world attacks
* Runs after deployment

---

## What is SCA?

Software Composition Analysis scans third-party dependencies for known vulnerabilities.

---

## Difference between SAST, DAST, and SCA

| Feature | SAST        | DAST                | SCA                |
| ------- | ----------- | ------------------- | ------------------ |
| Target  | Source code | Running application | Dependencies       |
| Stage   | Early       | Late                | Build              |
| Type    | White-box   | Black-box           | Component analysis |

---

# 3. Tools and Concepts

## What is CodeQL used for?

CodeQL is a SAST tool that analyzes source code using queries to detect vulnerabilities and security issues.

---

## Difference between CodeQL and SonarQube

| CodeQL                       | SonarQube                  |
| ---------------------------- | -------------------------- |
| Security-focused             | Code quality with security |
| Query-based analysis         | Rule-based analysis        |
| Deep vulnerability detection | Easier to use              |

---

## What is CVE?

CVE (Common Vulnerabilities and Exposures) is a unique identifier assigned to publicly known security vulnerabilities.

Example: CVE-2021-44228

---

# 4. SCA (Dependency Scanning)

## How do you detect vulnerable libraries?

* Scan dependency files such as pom.xml, package.json, or requirements.txt
* Compare versions with vulnerability databases
* Use tools like Snyk or OWASP Dependency-Check

---

# 5. DAST

## Why is DAST slower than SAST?

DAST is slower because:

* It runs on a live application
* Sends HTTP requests
* Waits for responses

SAST only analyzes code, so it is faster.

---

## Can DAST run without deployment?

No. DAST requires a running application, typically deployed in a staging or test environment.

---

# 6. Secrets Scanning

## What happens if API keys are pushed to Git?

* Unauthorized access
* Data breaches
* Financial loss due to misuse

---

## How do you prevent secrets leakage?

* Use secret scanning tools
* Avoid hardcoding secrets
* Use environment variables
* Use secret managers
* Rotate keys regularly

---

# 7. Container Security

## How do you scan Docker images?

```bash
trivy image my-app:latest
```

* Scans OS packages and dependencies
* Reports vulnerabilities based on known databases

---

## What is a base image vulnerability?

A base image vulnerability is a security issue present in the underlying image (such as Ubuntu or Alpine) that affects all derived container images.

---

# 8. CI/CD Security Integration

## When does SAST run in pipeline?

* Pre-commit stage (optional)
* Pull request stage
* Build stage (most common)

---

# 9. Pipeline-Based Questions

## Design a secure CI/CD pipeline

```text
Code Commit → Secrets Scan → SAST → SCA → Build → Container Scan → Deploy (Dev) → DAST → Approval → Production
```

---

## Where will you fail the build?

* Fail for:

  * Critical and High vulnerabilities
  * Secrets detected
* Allow Medium and Low (log and monitor)

---

## What thresholds will you set?

| Severity | Action |
| -------- | ------ |
| Critical | Fail   |
| High     | Fail   |
| Medium   | Warn   |
| Low      | Ignore |

---

## How do you handle false positives?

* Validate findings manually
* Suppress with proper justification
* Maintain allowlist
* Tune scanning rules

---

# 10. Scenario-Based Questions

## A vulnerability is found in production. What will you do?

1. Identify severity using CVSS
2. Analyze impact
3. Fix vulnerability or update dependency
4. Rebuild artifact or Docker image
5. Redeploy through pipeline
6. Monitor logs and systems

---

## Your pipeline is slow due to scans. What will you do?

* Run scans in parallel
* Use incremental scanning
* Scan only changed code
* Cache dependencies
* Run heavy scans separately

---

## Developer says security is blocking releases. What will you do?

* Define severity thresholds
* Allow temporary exceptions
* Educate developers on secure coding
* Implement shift-left security

---

# 11. Policy and Compliance

## What is CIS Benchmark?

CIS Benchmarks are security best practices for systems, cloud platforms, and applications provided by the Center for Internet Security.

---

## What is OWASP Top 10?

OWASP Top 10 is a list of the most critical web application security risks, such as:

* Injection
* Broken Authentication
* Security Misconfiguration

---

## What is Least Privilege Principle?

The least privilege principle means giving only the minimum access required to users or systems to perform their tasks.

---

# 12. Advanced Topics

## How do you secure Kubernetes?

* Use RBAC for access control
* Implement network policies
* Apply pod security standards
* Scan container images before deployment

---

## How do you secure Terraform or IaC?

* Scan configurations using security tools
* Avoid hardcoding secrets
* Use secure defaults and configurations

---

## What is Secrets Management?

Secrets management is the practice of securely storing and accessing sensitive data such as API keys and passwords using tools like Vault or cloud secret managers.

---

## What is Image Signing?

Image signing ensures that container images are trusted and have not been tampered with.

---

## What is Zero Trust?

Zero Trust is a security model where every request is authenticated and authorized, and no entity is trusted by default.

---

# 13. Sample CI/CD Pipeline (GitHub Actions)

```yaml
name: Secure CI/CD Pipeline

on: [push]

jobs:
  security:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Run SAST
        run: codeql analyze

      - name: Run SCA
        run: snyk test

      - name: Build Docker Image
        run: docker build -t my-app .

      - name: Scan Docker Image
        run: trivy image my-app

      - name: Run DAST
        run: zap-baseline.py -t http://test-app
```

---


## Secure-by-Design in SDLC (Q&A)

---

## 1. What is secure-by-design in SDLC?

**Answer:**

Secure-by-design means integrating security practices from the beginning of the Software Development Life Cycle instead of adding them at the end. It includes defining security requirements, performing threat modeling during design, enforcing secure coding practices, and automating security testing in CI/CD pipelines.

---

## 2. How do you embed security into each phase of SDLC?

**Answer:**

In the requirement phase, I define security requirements such as authentication, authorization, and encryption.
In the design phase, I perform threat modeling using tools like Microsoft Threat Modeling Tool.
During development, I enforce secure coding practices and use SAST tools.
In testing, I integrate SAST, DAST, and SCA scans.
In deployment, I ensure secure configurations and proper secrets management.

---

## 3. How do you collaborate with developers to improve security?

**Answer:**

I collaborate by integrating security tools directly into the developer workflow, providing secure coding guidelines, and conducting awareness sessions. I also ensure developers receive early feedback through CI/CD pipelines so they can fix issues quickly.

---

## 4. Developers say security slows down development. How do you handle it?

**Answer:**

I automate security checks in CI/CD pipelines and set severity thresholds so only critical issues block builds. I also shift security earlier in the process to reduce rework and provide faster feedback.

---

## 5. What is threat modeling and why is it important?

**Answer:**

Threat modeling is the process of identifying potential security threats during the design phase. It helps proactively address vulnerabilities before development begins, reducing both risk and cost.

---

## 6. What is STRIDE?

**Answer:**

STRIDE is a threat modeling framework that stands for Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege. It is used to categorize and identify potential threats in a system.

---

## 7. How do you ensure developers follow secure coding practices?

**Answer:**

I provide secure coding standards, enforce code reviews, and integrate tools like SonarQube into CI pipelines to detect vulnerabilities early.

---

## 8. How do you manage secrets securely?

**Answer:**

I avoid hardcoding secrets in source code and use secure storage solutions like HashiCorp Vault. Secrets are injected at runtime and rotated regularly.

---

## 9. How do you collaborate with platform or DevOps teams?

**Answer:**

I work with platform teams to secure CI/CD pipelines, enforce Infrastructure-as-Code security practices, implement proper access controls, and ensure monitoring and logging are in place.

---

## 10. Scenario: A team is not following security practices. What will you do?

**Answer:**

I first identify the challenges faced by the team, then provide training and simplify security processes. I introduce automated checks and make security part of the Definition of Done to ensure compliance.

---

## 11. Scenario: No security exists in SDLC. How will you start?

**Answer:**

I begin with a risk assessment, then introduce SAST and SCA into CI/CD pipelines. I implement threat modeling in the design phase and gradually enforce policies and automation without disrupting development speed.

---

## 12. What are security gates in CI/CD?

**Answer:**

Security gates are checkpoints in the pipeline where builds fail if vulnerabilities exceed a defined severity threshold. This ensures that only secure code progresses to deployment.

---

## 13. What is Definition of Done with security?

**Answer:**

It means a feature is considered complete only after passing all security checks, including code scans, dependency checks, and adherence to secure coding standards.

---

## 14. How do you balance security and speed?

**Answer:**

By automating security processes, shifting security earlier in the lifecycle, running scans in parallel, and focusing on high-risk vulnerabilities, I ensure strong security without slowing down delivery.

---

## 15. Explain how you implemented secure SDLC in a project

**Answer:**

In my project, I integrated SAST and SCA tools into CI/CD pipelines, conducted threat modeling during the design phase, enforced secure coding practices, and implemented container scanning before deployment. I collaborated with developers through regular feedback and training, ensuring security became an integral part of the development lifecycle.

---



---
