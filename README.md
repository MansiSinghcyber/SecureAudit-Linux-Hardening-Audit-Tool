# SecureAudit-Linux-Hardening-Audit-Tool
# 🛡️ SecureAudit — Linux Hardening Audit Tool

> **A Python-based Linux security auditing tool that evaluates essential system hardening controls, identifies security weaknesses, calculates a security score, and generates actionable hardening recommendations.**

---

## 📌 Project Overview

**SecureAudit** is a lightweight Linux Hardening Audit Tool developed in Python to perform automated security checks on a Linux system.

The tool evaluates multiple security controls including:

* Firewall configuration
* Privileged accounts
* SSH root-login configuration
* Critical file permissions
* Listening network ports
* Pending system updates
* Running system services
* Basic rootkit indicators

After performing the audit, SecureAudit generates:

* Detailed security findings
* PASS / WARNING / FAIL / INFO classifications
* Hardening recommendations
* Security score out of 100
* Overall risk level
* Timestamped audit report

The project demonstrates practical Linux security auditing, system hardening, Python automation, security assessment, and report generation.

---

# 🎯 Objectives

| Objective            | Description                                                 |
| -------------------- | ----------------------------------------------------------- |
| 🔍 Security Auditing | Automatically inspect important Linux security controls     |
| 🛡️ Hardening        | Identify configuration weaknesses and recommend remediation |
| 👤 Access Control    | Check privileged accounts and SSH root-login configuration  |
| 🔥 Network Security  | Evaluate firewall status and listening ports                |
| 📁 File Security     | Check permissions of sensitive files                        |
| 🔄 Patch Management  | Detect available package updates                            |
| ⚙️ Service Security  | Identify the number of running services                     |
| 🔎 Threat Detection  | Perform basic rootkit-indicator checks                      |
| 📊 Risk Assessment   | Calculate an overall security score                         |
| 📄 Reporting         | Generate timestamped audit reports                          |

---

# ✨ Features

### 1. System Information

Collects basic information about the audited Linux system:

* Operating system
* Kernel release
* Hostname

### 2. Firewall Audit

Checks whether UFW is:

* Installed
* Active
* Inactive
* Unable to be verified

### 3. Privileged Account Audit

Checks `/etc/passwd` for accounts with:

```text
UID = 0
```

The tool verifies whether accounts other than `root` have UID 0.

### 4. SSH Security Audit

Checks the SSH configuration for:

```text
PermitRootLogin
```

The tool identifies whether root login is explicitly restricted.

### 5. Critical File Permission Audit

Checks permissions of:

```text
/etc/shadow
```

This file contains sensitive authentication information and should have restricted permissions.

### 6. Listening Port Audit

Uses Linux socket information to identify listening network sockets.

### 7. System Update Audit

Checks whether packages are available for upgrade.

### 8. Running Service Audit

Checks currently running system services and identifies systems with a high number of active services for further review.

### 9. Basic Rootkit Indicator Audit

Checks selected hidden/suspicious paths for basic indicators.

> **Note:** This is an indicator check and is not a full rootkit or malware detection engine.

---

# 🏗️ System Architecture

```text
                    ┌─────────────────────────┐
                    │      Linux System       │
                    │                         │
                    │  Configuration / Files  │
                    │  Services / Networking  │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │       SecureAudit        │
                    │       Python Tool        │
                    └────────────┬────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
              ▼                  ▼                  ▼
        System Checks      Security Checks    Threat Checks
              │                  │                  │
              ▼                  ▼                  ▼
        System Info          Firewall             Rootkit
        Kernel               SSH                  Indicators
        Hostname             Accounts
                             Permissions
                             Ports
                             Updates
                             Services
              │                  │                  │
              └──────────────────┼──────────────────┘
                                 ▼
                    ┌─────────────────────────┐
                    │   Finding Classification │
                    │                         │
                    │ PASS / WARNING / FAIL   │
                    │ INFO                    │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │   Security Score Engine │
                    │                         │
                    │       0 – 100           │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Risk Classification  │
                    │                         │
                    │ LOW / MEDIUM / HIGH /  │
                    │ CRITICAL                │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │      Report Generator   │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │ reports/security_*.txt  │
                    └─────────────────────────┘
```

---

# 🔄 Audit Workflow

```text
START
  │
  ▼
Collect System Information
  │
  ▼
Check Firewall
  │
  ▼
Check Privileged Accounts
  │
  ▼
Check SSH Configuration
  │
  ▼
Check Critical File Permissions
  │
  ▼
Check Listening Ports
  │
  ▼
Check Pending Updates
  │
  ▼
Check Running Services
  │
  ▼
Check Basic Rootkit Indicators
  │
  ▼
Classify Findings
  │
  ├── PASS
  ├── WARNING
  ├── FAIL
  └── INFO
  │
  ▼
Calculate Security Score
  │
  ▼
Determine Risk Level
  │
  ▼
Generate Recommendations
  │
  ▼
Generate Timestamped Report
  │
  ▼
END
```

---

# 🔐 Security Audit Controls

|  # | Audit Control       | What is Checked           | Possible Result       |
| -: | ------------------- | ------------------------- | --------------------- |
|  1 | System Information  | OS, kernel, hostname      | INFO                  |
|  2 | Firewall            | UFW installation/status   | PASS / WARNING        |
|  3 | Privileged Accounts | UID 0 accounts            | PASS / WARNING        |
|  4 | SSH Security        | `PermitRootLogin`         | PASS / WARNING / FAIL |
|  5 | `/etc/shadow`       | File permissions          | PASS / WARNING        |
|  6 | Open Ports          | Listening sockets         | PASS / INFO           |
|  7 | System Updates      | Upgradeable packages      | PASS / WARNING        |
|  8 | Running Services    | Active services           | PASS / WARNING        |
|  9 | Rootkit Indicators  | Selected suspicious paths | PASS / WARNING        |

---

# 📊 Finding Classification

| Status     | Meaning                                            |
| ---------- | -------------------------------------------------- |
| ✅ PASS     | Security control meets the defined audit condition |
| ⚠️ WARNING | Potential security weakness requiring review       |
| ❌ FAIL     | Significant security control failure               |
| ℹ️ INFO    | Informational system/audit information             |

---

# 🎯 Security Scoring Methodology

SecureAudit uses a simple weighted scoring model to provide an overall indication of the Linux system's hardening posture.

The audit starts with a maximum score of:

```text
100 / 100
```

Points are deducted when a security weakness is detected. The final score is never allowed to fall below `0`.

```text
Final Score = max(100 − Total Deductions, 0)
```

## 📊 Actual Deduction Logic

The following deductions are implemented in the current version of `secure_audit.py`:

| Audit Check               | Condition                                   | Status  | Deduction |
| ------------------------- | ------------------------------------------- | ------- | --------: |
| Firewall                  | UFW is not installed                        | WARNING |       −10 |
| Firewall                  | UFW is installed but inactive               | WARNING |       −10 |
| Firewall                  | Firewall status cannot be confirmed         | WARNING |       −10 |
| Privileged Accounts       | Additional UID 0 accounts detected          | WARNING |       −15 |
| SSH Root Login            | `PermitRootLogin yes`                       | FAIL    |       −15 |
| SSH Root Login            | `PermitRootLogin` not explicitly configured | WARNING |        −5 |
| `/etc/shadow` Permissions | Permissions are not `0600` or `0640`        | WARNING |       −15 |
| `/etc/shadow`             | Permission denied during audit              | WARNING |        −5 |
| Open Ports                | No listening ports detected                 | PASS    |         0 |
| Open Ports                | Listening ports detected                    | INFO    |         0 |
| System Updates            | Package updates available                   | WARNING |       −10 |
| System Updates            | No pending updates                          | PASS    |         0 |
| Running Services          | 11–20 running services                      | WARNING |        −5 |
| Running Services          | More than 20 running services               | WARNING |       −10 |
| Rootkit Indicators        | Suspicious predefined paths detected        | WARNING |       −20 |
| Rootkit Indicators        | No suspicious predefined paths detected     | PASS    |         0 |
| System Information        | Informational system details                | INFO    |         0 |

---

## 🧮 Example: Initial Audit

During the initial audit, SecureAudit produced:

```text
Starting Score = 100
```

The following findings resulted in deductions:

### 1. Firewall

```text
UFW was not installed
Deduction = −10
```

Remaining score:

```text
100 − 10 = 90
```

### 2. SSH Root Login

```text
PermitRootLogin was not explicitly configured
Deduction = −5
```

Remaining score:

```text
90 − 5 = 85
```

### 3. System Updates

```text
88 package updates were available
Deduction = −10
```

Remaining score:

```text
85 − 10 = 75
```

### 4. Running Services

The audit detected:

```text
20 running services
```

The current implementation assigns:

```text
11–20 running services = −5
```

Therefore:

```text
75 − 5 = 70
```

### Final Initial Score

```text
100
 −10  Firewall
  −5  SSH
 −10  Updates
  −5  Running Services
─────
  70 / 100
```

Therefore:

```text
Security Score: 70/100
Risk Level: MEDIUM
```

---

# 🛡️ Before vs After Firewall Hardening

The project also demonstrates how the score changes after a hardening action.

## Before

UFW was not installed:

```text
Firewall = WARNING
Deduction = −10

Security Score = 70/100
```

## Hardening Action

UFW was installed and enabled:

```bash
sudo apt install ufw -y
sudo ufw enable
sudo ufw status verbose
```

The firewall subsequently reported:

```text
Status: active
Default: deny (incoming), allow (outgoing)
```

SecureAudit then classified the firewall as:

```text
[PASS] Firewall Status
Details: UFW firewall is active.
```

Because a PASS finding has no deduction:

```text
Firewall Deduction = 0
```

The score therefore increased by 10 points:

```text
70 + 10 = 80
```

## After

```text
PASS:     5
WARNING:  3
FAIL:     0
INFO:     1

Security Score: 80/100
Risk Level: MEDIUM
```

### Score Improvement

```text
BEFORE                    AFTER
70/100                    80/100
  │                         │
  │ +10 points              │
  └───────────────►         │
                            │
                     Firewall = PASS
```

---

# ⚠️ Important Interpretation

The SecureAudit score is a **project-specific weighted security indicator**.

It is **not**:

* a CVSS score
* a CIS Benchmark compliance score
* an ISO 27001 compliance score
* a professional vulnerability rating
* a guarantee that the system is secure

A higher score means that fewer of the security weaknesses covered by the implemented checks were detected.

The score should therefore be interpreted together with the individual findings and recommendations.

---

# 📈 Risk-Level Mapping

After deductions are applied, SecureAudit maps the final score to a risk category:

| Final Score | Risk Level |
| ----------: | ---------- |
|      90–100 | LOW        |
|       70–89 | MEDIUM     |
|       40–69 | HIGH       |
|        0–39 | CRITICAL   |

The mapping is implemented by the `determine_risk_level()` function.

```text
100 ───────────────────── 90
          LOW

89 ────────────────────── 70
         MEDIUM

69 ────────────────────── 40
          HIGH

39 ─────────────────────── 0
        CRITICAL
```

---

# 🔄 Scoring Process

```text
                START
                  │
                  ▼
          Initial Score = 100
                  │
                  ▼
          Run Security Checks
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
      PASS     WARNING     FAIL
        │         │         │
      −0       Deduct      Deduct
        │       points      points
        └─────────┼─────────┘
                  ▼
        Calculate Total Deductions
                  │
                  ▼
     Final Score = 100 − Deductions
                  │
                  ▼
       Prevent Score Below Zero
                  │
                  ▼
        Determine Risk Level
                  │
                  ▼
          Generate Report
                  │
                  ▼
                 END
```

---

## 💡 Design Rationale

The scoring model intentionally gives greater deductions to controls considered more significant to system hardening.

For example:

```text
Rootkit Indicator        −20
Privileged Accounts      −15
SSH Root Login           −15
Shadow Permissions       −15
Firewall                 −10
System Updates           −10
Running Services       −5/−10
```

This creates a simple weighted model where potentially more significant security weaknesses have a greater effect on the overall score.

The model can be expanded in future versions by introducing additional controls, severity weighting, compliance benchmarks, and configurable scoring policies.


---

# 🧪 Before vs After Hardening Demonstration

The project includes a practical firewall-hardening demonstration.

## 🔴 Before Hardening

Initial audit:

```text
Firewall Status: WARNING
UFW: Not installed

PASS: 4
WARNING: 4
FAIL: 0
INFO: 1

Security Score: 70/100
Risk Level: MEDIUM
```

---

## 🛠️ Hardening Action

UFW was installed:

```bash
sudo apt install ufw -y
```

The firewall was enabled:

```bash
sudo ufw enable
```

Firewall configuration was verified:

```bash
sudo ufw status verbose
```

Result:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing)
```

---

## 🟢 After Hardening

SecureAudit was executed again using administrative privileges:

```bash
sudo python3 secure_audit.py
```

Result:

```text
Firewall Status: PASS
Details: UFW firewall is active.

PASS: 5
WARNING: 3
FAIL: 0
INFO: 1

Security Score: 80/100
Risk Level: MEDIUM
```

### Improvement

| Metric         |     Before |      After |   Change |
| -------------- | ---------: | ---------: | -------: |
| Security Score |     70/100 | **80/100** |  **+10** |
| PASS           |          4 |      **5** |       +1 |
| WARNING        |          4 |      **3** |       -1 |
| FAIL           |          0 |          0 |        — |
| INFO           |          1 |          1 |        — |
| Firewall       | ⚠️ Warning |     ✅ Pass | Improved |

---

# 🖥️ Example Audit Output

```text
============================================================
      SECUREAUDIT - LINUX HARDENING AUDIT TOOL
============================================================

[+] Collecting system information...
[+] Checking firewall status...
[+] Checking privileged accounts...
[+] Checking SSH security configuration...
[+] Checking /etc/shadow permissions...
[+] Checking open listening ports...
[+] Checking for pending system updates...
[+] Checking running services...
[+] Checking basic rootkit indicators...

============================================================
        SECUREAUDIT - LINUX HARDENING AUDIT REPORT
============================================================

[PASS] Firewall Status
Details: UFW firewall is active.

[PASS] Privileged Accounts
Details: Only the root account has UID 0.

[WARNING] SSH Root Login
Details: PermitRootLogin is not explicitly configured.

[PASS] /etc/shadow Permissions
Details: /etc/shadow permissions are secure.

[PASS] Open Ports
Details: No listening ports detected.

[WARNING] System Updates
Details: Package updates are available.

[WARNING] Running Services
Details: Running services require review.

[PASS] Basic Rootkit Indicators
Details: No suspicious paths detected.

============================================================
AUDIT SUMMARY
============================================================
PASS: 5
WARNING: 3
FAIL: 0
INFO: 1

============================================================
FINAL SECURITY ASSESSMENT
============================================================
Security Score: 80/100
Risk Level: MEDIUM
============================================================
```

---

# 📁 Project Structure

```text
Linux-Hardening-Audit-Tool/
│
├── secure_audit.py
├── README.md
│
├── reports/
│   ├── security_report.txt
│   ├── security_report_2026-08-30_23-24-15.txt
│   ├── security_report_2026-08-30_23-27-54.txt
│   ├── security_report_2026-08-30_23-33-05.txt
│   └── security_report_2026-08-30_23-34-21.txt
│
└── screenshots/
    ├── before_audit.png
    ├── firewall_hardening.png
    └── after_audit.png
```

---

# ⚙️ Requirements

| Requirement         | Purpose                                |
| ------------------- | -------------------------------------- |
| Linux               | Target operating system                |
| Python 3            | Execute the audit tool                 |
| UFW                 | Firewall auditing                      |
| systemd / systemctl | Service auditing                       |
| `ss`                | Listening-port auditing                |
| APT                 | Package-update auditing                |
| Root privileges     | Access to protected system information |

The project was developed and tested in a Kali Linux environment.

---

# 🚀 Installation

## 1. Clone the repository

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
```

## 2. Enter the project directory

```bash
cd Linux-Hardening-Audit-Tool
```

## 3. Verify Python

```bash
python3 --version
```

## 4. Verify the script

```bash
python3 -m py_compile secure_audit.py
```

If no output is displayed, the Python syntax check passed.

---

# ▶️ Usage

Because SecureAudit performs system-level security checks, run it with administrative privileges:

```bash
sudo python3 secure_audit.py
```

The tool will automatically:

```text
Audit System
     ↓
Analyze Security Controls
     ↓
Classify Findings
     ↓
Calculate Score
     ↓
Generate Recommendations
     ↓
Save Report
```

---

# 📄 Generated Reports

Reports are automatically stored inside:

```text
reports/
```

Example:

```text
security_report_2026-08-30_23-34-21.txt
```

Timestamped reports allow multiple audits to be preserved instead of overwriting previous results.

---

# 🛡️ Hardening Recommendations Generated

Depending on the audit results, SecureAudit can recommend actions such as:

```bash
sudo apt install ufw
```

```bash
sudo ufw enable
```

```bash
sudo apt update && sudo apt upgrade
```

SSH configuration review:

```text
/etc/ssh/sshd_config
```

Service review:

```bash
systemctl list-units --type=service --state=running
```

> Remediation commands should be reviewed before execution because security configuration changes can affect system functionality.

---

# 🔬 Testing Methodology

The project follows a simple security-audit lifecycle:

```text
1. Establish Baseline
        ↓
2. Run Automated Audit
        ↓
3. Identify Weaknesses
        ↓
4. Apply Safe Hardening
        ↓
5. Re-run Audit
        ↓
6. Compare Results
        ↓
7. Generate Evidence
```

The firewall configuration was used as the practical hardening demonstration.

---

# 📸 Evidence

The `screenshots/` directory contains evidence of the project's execution.

Recommended evidence includes:

| Screenshot               | Purpose                            |
| ------------------------ | ---------------------------------- |
| `before_audit.png`       | Shows initial 70/100 baseline      |
| `firewall_hardening.png` | Shows UFW activation/configuration |
| `after_audit.png`        | Shows improved 80/100 assessment   |

---

# ⚠️ Limitations

SecureAudit is intentionally designed as a lightweight educational security auditing tool.

### Current limitations

* It does not perform a complete vulnerability assessment.
* It does not replace professional vulnerability scanners.
* Rootkit detection is limited to predefined indicators.
* Running-service analysis is based primarily on service count and requires further manual review.
* The security score is project-specific rather than an industry-standard risk score.
* SSH analysis currently focuses on root-login configuration.
* The tool does not perform automated remediation.
* It does not provide complete compliance certification.
* It does not guarantee that a system is completely secure.

---

# 🔮 Future Improvements

Potential future enhancements include:

```text
SecureAudit
│
├── Advanced SSH Auditing
│
├── Password Policy Analysis
│
├── File Integrity Monitoring
│
├── CIS Benchmark Mapping
│
├── Lynis Integration
│
├── CVE / Vulnerability Detection
│
├── JSON / CSV Reports
│
├── HTML Security Dashboard
│
├── Email Notifications
│
├── Automated Remediation
│
└── SIEM Integration
```

---

# 🎓 Learning Outcomes

This project demonstrates practical experience with:

* Linux administration
* Linux security hardening
* Python scripting
* System command execution
* File permission auditing
* User and privilege analysis
* Firewall configuration
* SSH security
* Network socket analysis
* Service management
* Patch management
* Security scoring
* Security reporting
* Basic threat indicators
* Cybersecurity automation

---

# 🧠 Key Security Concepts Demonstrated

```text
                Linux Security
                      │
        ┌─────────────┼─────────────┐
        │             │             │
   Access Control  Network       System
        │          Security       Security
        │             │             │
   UID 0 Users      UFW          Updates
   SSH Root         Ports        Services
   Permissions                    Files
        │                           │
        └─────────────┬─────────────┘
                      │
                      ▼
                Security Audit
                      │
                      ▼
                Risk Assessment
                      │
                      ▼
                Hardening Action
                      │
                      ▼
                Re-Audit
```

---

# 🏁 Conclusion

**SecureAudit** provides a lightweight and automated approach to Linux security auditing.

The project demonstrates how Python can be used to collect system information, inspect security configurations, identify potential weaknesses, classify findings, calculate a security score, generate hardening recommendations, and produce persistent audit reports.

The before-and-after firewall demonstration shows how a security configuration change can be validated through automated re-assessment:

```text
BEFORE
70/100
MEDIUM
     │
     ▼
Firewall Hardening
     │
     ▼
AFTER
80/100
MEDIUM
```

The project serves as a practical foundation for extending Linux security auditing toward more advanced vulnerability assessment, compliance automation, security dashboards, and automated remediation.

---

# 📌 Quick Command Reference

```bash
# Enter project
cd Linux-Hardening-Audit-Tool

# Check Python version
python3 --version

# Validate Python syntax
python3 -m py_compile secure_audit.py

# Run SecureAudit
sudo python3 secure_audit.py

# View generated reports
ls reports

# View latest report
cat reports/security_report_*.txt

# Check firewall
sudo ufw status verbose

# View running services
systemctl list-units --type=service --state=running

# View listening ports
ss -tuln
```

---

