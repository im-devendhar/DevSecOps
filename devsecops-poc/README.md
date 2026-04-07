# DevSecOps Pipeline Implementation (POC)

## 1. Project Overview

This project demonstrates the implementation of a complete DevSecOps pipeline integrating security at every stage of the Software Development Life Cycle (SDLC).

The pipeline includes Static Application Security Testing (SAST), Software Composition Analysis (SCA), Secrets Scanning, Container Security, and Dynamic Application Security Testing (DAST).

---

## 2. Architecture Overview

Code → SAST → SCA → Secrets Scan → Build → Container Scan → Deploy → DAST

---

## 3. Tools Used

* SonarQube – Static Code Analysis (SAST)
* Snyk – Dependency Vulnerability Scanning (SCA)
* Gitleaks – Secrets Detection
* Docker – Containerization
* Trivy – Container Image Scanning
* OWASP ZAP – Dynamic Security Testing (DAST)
* GitHub Actions – CI/CD Pipeline
* AWS EC2 – Deployment Environment

---

## 4. Implementation Steps

### 4.1 Source Code Setup

* Created frontend application
* Organized project structure
* Added configuration files (package.json, Dockerfile, sonar-project.properties)

---

### 4.2 SAST Implementation (SonarQube)

* Installed SonarQube on AWS EC2 using Docker
* Created project and generated authentication token
* Configured sonar-project.properties
* Executed scan using sonar-scanner

**Outcome:**

* Code quality analyzed
* Security vulnerabilities identified (if any)
* Quality gate evaluation performed

---

### 4.3 SCA Implementation (Snyk)

* Installed Snyk CLI
* Authenticated using API token
* Executed:

  * snyk test
  * snyk monitor

**Outcome:**

* Dependency vulnerabilities analyzed
* Project uploaded to Snyk dashboard
* Continuous monitoring enabled

---

### 4.4 Secrets Scanning (Gitleaks)

* Installed Gitleaks using Docker
* Performed filesystem scan using:
  --no-git option

**Outcome:**

* No hardcoded secrets detected
* Ensured secure handling of credentials

---

### 4.5 Containerization (Docker)

* Created Dockerfile for application
* Built Docker image:
  docker build -t devsecops-app .
* Ran container:
  docker run -p 3000:80 devsecops-app

**Outcome:**

* Application successfully containerized
* Accessible via browser

---

### 4.6 Container Security (Trivy)

* Installed Trivy
* Scanned Docker image:
  trivy image devsecops-app

**Outcome:**

* Identified vulnerabilities in base image libraries (e.g., libpng, zlib)
* Recommended updating base image

---

### 4.7 Deployment (AWS EC2)

* Deployed application on EC2 instance
* Configured security groups (ports 3000, 9000)
* Verified accessibility via public IP

---

### 4.8 DAST Implementation (OWASP ZAP)

* Executed ZAP baseline scan using Docker
* Generated HTML report

**Findings:**

* Missing security headers (CSP, X-Frame-Options, etc.)
* Server information disclosure
* Cache-related warnings

---

## 5. CI/CD Pipeline (GitHub Actions)

* Created workflow file (.github/workflows/devsecops.yml)
* Integrated:

  * SonarQube scan
  * Snyk monitor
  * Gitleaks scan
  * Docker build
  * Trivy scan
  * OWASP ZAP scan

**Outcome:**

* Automated security checks on every code push
* Continuous security validation

---

## 6. Key Security Findings

* Container vulnerabilities due to outdated base image
* Missing HTTP security headers
* Minor information disclosure issues

---

## 7. Recommendations

* Use updated base images
* Add security headers in Nginx configuration
* Enforce pipeline failure on high/critical vulnerabilities
* Implement HTTPS for secure communication

---

## 8. Conclusion

This POC successfully demonstrates the integration of security practices into the CI/CD pipeline, ensuring early detection and mitigation of vulnerabilities.

The pipeline aligns with DevSecOps principles by shifting security left and automating security checks.

---

## 9. Future Enhancements

* Enforce Quality Gates (SonarQube)
* Fail pipeline on vulnerabilities (Trivy, Snyk)
* Add Infrastructure as Code (Terraform + Checkov)
* Integrate Slack/Email notifications
* Automate deployment to AWS

---
