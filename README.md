# Task C: Automated Security Testing

## 1. CI/CD Pipeline Configuration

A Continuous Integration/Continuous Deployment (CI/CD) pipeline was implemented using GitHub Actions to automate security testing of the web application.

The workflow is triggered automatically on every push to the repository and executes on a GitHub-hosted Ubuntu latest runner. The pipeline integrates Static Application Security Testing (SAST) using Semgrep to analyse the source code for security vulnerabilities.

The pipeline is defined in `.github/workflows/security_scan.yml` and performs the following steps:

- Checks out the repository source code using GitHub Actions
- Installs Semgrep security analysis tool via pip
- Executes Semgrep against the `webapp/` directory using an automated rule configuration (`--config=auto`)
- Generates a structured text report of detected vulnerabilities
- Uploads the scan results as a build artifact, which can be downloaded and reviewed for analysis and reporting purposes

This implementation demonstrates a DevSecOps approach by integrating automated security testing directly into the development lifecycle, ensuring that vulnerabilities are identified early in the codebase before deployment.

---

## 2. Automated Scan Results and Evidence

![Pipeline Execution](screenshots/pipeline_execution.png)

The screenshot above shows the successful completion of the GitHub Actions workflow. The green checkmark confirms that the semgrep-scan job finished without errors, and the "Artifacts" section at the bottom shows that the semgrep-report was successfully generated. This proves that the automation logic in the YAML file is fully operational.

![Scan Results](screenshots/results.png)

The screenshot above shows the actual output contained within the `scan-results.txt` file. As shown in the summary table, the scanner identified 91 code findings. The results are categorized by severity, with a significant number of "Blocking" issues identified. These findings provide a roadmap for the manual exploitation phase, specifically highlighting critical vulnerabilities such as SQL Injection and Cross-Site Scripting (XSS) within the `app.py` file.
 
 ---
 ## 3. Triage of Results and False Positives

After the automated scan was completed, the scan results were manually reviewed to separate real security findings from results that didn't matter. Given the high volume of 91 findings, the triage process focused on finding patterns and picking out high-priority alerts rather than doing a full line-by-line analysis of every single finding.

The triage process involved the following steps:

### Review of Framework-Related Findings
A big part of the report was made up of repeated warnings about the database and framework settings (like Flask-SQLAlchemy). After checking a few examples of these, it was clear that many were just standard ways the app is built and not real vulnerabilities. These findings were therefore set aside.

### Prioritization of Relevant Findings
The scan results included several "audit-level" warnings, which are only risks depending on how the code is used. These were reviewed and ignored in favor of findings that showed much clearer coding mistakes.

### Identification of High-Risk Issues
The analysis focused on findings that showed real security weaknesses in the application. Specifically, patterns like unsafe SQL queries and bad handling of user input were picked out as the main concerns, leading to the SQL Injection and Cross-Site Scripting (XSS) risks.

This triage process made sure that the next analysis in Task D focuses on the most important security issues found by the scan, while reducing the noise from minor findings.


