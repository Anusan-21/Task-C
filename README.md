# Task C: Automated Security Testing

## 1. CI/CD Pipeline Configuration

A Continuous Integration/Continuous Deployment (CI/CD) pipeline was implemented using GitHub Actions to automate security testing of the web application.

The workflow is triggered automatically on every push to the repository and executes on a GitHub-hosted Ubuntu latest runner. The pipeline integrates Static Application Security Testing (SAST) using Semgrep to analyse the source code for security vulnerabilities.

The pipeline is defined in `.github/workflows/security_scan.yml` and performs the following steps:

- Checks out the repository source code using GitHub Actions
- Installs Semgrep security analysis tool via pip
- Executes Semgrep against the `webapp/` directory using an automated rule configuration (`--config=auto`)
- Generates a structured JSON report of detected vulnerabilities
- Uploads the scan results as a build artifact, which can be downloaded and reviewed for analysis and reporting purposes

This implementation demonstrates a DevSecOps approach by integrating automated security testing directly into the development lifecycle, ensuring that vulnerabilities are identified early in the codebase before

---



