# Incident-Response-Framework
Comprehensive incident response planning and execution framework for cybersecurity teams.

# 1. Incident Response Report: DDoS Attack on Web Infrastructure
# 2. Incident Response Report: Phishing Attack on Banking Staff
# 3. Incident Response Report: Data Breach at Government Hospital 


<br><br>


# Incident Response Report: DDoS Attack on Web Infrastructure
### Date of Incident: April 25, 2024
### Analyst: Hacker9 (Cybersecurity Analyst)
### Company: RTP IT Solutions Ltd  

<br><br>

## 1. Executive Summary
On April 25, 2024, RTP IT Solutions experienced a DDoS attack that disrupted access to its primary web application, causing downtime and degraded performance for approximately 2 hours. The attack was identified by abnormal traffic patterns and triggered alerts from our SIEM and cloud-based monitoring systems.

The incident was contained, mitigated, and systems were fully restored. No data breach or internal compromise was detected. Post-incident actions have been recommended to strengthen defenses against future DDoS attempts. 

<br><br>

## 2. Incident Identification
### Detection Time:
+ April 25, 2024 — 14:03 PM 

### How it was Detected:
+ Alerts from Cloudflare (WAF & CDN): Spike in inbound HTTP requests.
+ Zabbix: Resource alerts from web server (CPU at 97%, memory usage spike).
+ Elastic SIEM: Anomaly detection triggered on network flow logs.

### Observed Indicators:
+ 250,000 requests/minute from globally distributed IPs
+ Targeted endpoints: /api/login, /index.php, /healthcheck
+ Traffic originated from over 3000 unique IPs across multiple regions

 <br><br>

##  3. Scope and Impact Assessment

| **Area**           | **Details**                                                                                       |
|--------------------|---------------------------------------------------------------------------------------------------|
| Affected Systems   | - Web server (Apache + PHP) hosted on AWS EC2  <br> - Load balancer (AWS ELB)                     |
| Attack Type        | Layer 7 HTTP Flood (application-level DDoS)                                                       |
| Duration           | ~2 hours (14:03 PM – 16:07 PM)                                                                     |
| User Impact        | ~80% of users experienced failed login or timeout                                                 |
| Data Loss          | No evidence of data breach or data integrity issues                                               |
| Business Impact    | Temporary downtime, loss of customer trust, SLA violations with 2 clients                         |

<br><br>

## 4. Containment Actions

| **Time** | **Action Taken**                                                                                   |
|----------|-----------------------------------------------------------------------------------------------------|
| 14:10    | Engaged Incident Response Team and declared Severity 2 incident                                     |
| 14:15    | Enabled Cloudflare Under Attack Mode                                                                 |
| 14:17    | Updated WAF rules to block IPs by behavior signature                                                 |
| 14:25    | Rate-limiting rules deployed on `/api/login` endpoint                                                |
| 14:40    | Blocked top offending IP ranges at the AWS NACL level                                                |
| 15:00    | Isolated non-critical services from the main web cluster                                             |
| 15:45    | Normal traffic restored, server resources normalized                                                 |

<br><br>

## 5. Eradication and Recovery
### Actions Performed:
+ Rebooted and rebalanced EC2 instances to clear exhausted system resources
+ Patched web application endpoint to better handle concurrent connections
+ Reviewed Cloudflare and AWS logs for suspicious follow-on activity
+ Confirmed no persistence mechanisms or malware installed

### Recovery Time:
+ Full recovery achieved by 16:07 PM

<br><br>

## 6. Root Cause Analysis
#### Root Cause:
The attack exploited the unauthenticated /api/login endpoint with a large-scale HTTP flood using a botnet.

#### Contributing Factors:
+ Lack of advanced rate-limiting or CAPTCHA on login
+ Cloudflare DDoS protection rules were too permissive
+ No prior load testing for DDoS resilience

<br><br>

## 7. Lessons Learned

| **Area**            | **Finding**                                 | **Action Item**                                  |
|---------------------|---------------------------------------------|--------------------------------------------------|
| Infrastructure      | WAF rules insufficient                      | Review and harden all WAF rules                  |
| Monitoring          | Initial alerts from Zabbix were missed      | Enhance escalation rules in SIEM                 |
| Resilience Testing  | No DDoS tabletop or load test ran recently  | Schedule regular DDoS simulations                |
| Application Design  | API login allowed unauthenticated flood     | Add CAPTCHA and exponential backoff              |

<br><br>

## 8. Recommended Improvements
### Short-Term (Next 7 Days):
+ Enable Cloudflare Bot Management on all endpoints
+ Implement CAPTCHA and rate-limiting on /api/login
+ Update runbooks with DDoS-specific playbook steps

### Long-Term:
+ Schedule quarterly tabletop DDoS simulations
+ Evaluate migration to auto-scaling serverless architecture
+ Explore commercial anti-DDoS appliances if attacks increase

<br><br>

## 9. Final Report Submission
**Status:** Closed <br><br>
**Submitted to:** CIO, SOC Manager, Legal Team <br><br>
**Retention:** Stored in internal SharePoint IR case folder <br><br>
**Attachments:**
+ Firewall/WAF logs
+ SIEM alert screenshots
+ Timeline spreadsheet (CSV)
+ Executive report (PDF)


<br><br>


# Incident Response Report: Phishing Attack on Banking Staff
### Date of Incident: April 15, 2024
### Analyst: Hacker9 (Cybersecurity Analyst)
### Organization: SecureBank PLC

<br><br>

## 1. Executive Summary

On April 15, 2024, SecureBank experienced a targeted phishing campaign that successfully tricked one employee into entering their credentials into a spoofed login page. The attacker used these credentials to access internal services for a brief period (approx. 21 minutes) before access was revoked.

Swift identification, containment, and response actions prevented any unauthorized transactions or data exfiltration. Regulatory reporting was completed, and incident documentation has been preserved per compliance requirements.

<br><br>

## 2. Identification
### Detection Time:
+ April 15, 2024 – 09:47 AM
### Detection Source:
+ Alert from Microsoft Defender for Office 365: Link-clicked from suspicious domain.
+ SIEM (Splunk) flagged abnormal login from geolocation inconsistent with user's profile.
+ User reported email after noticing anomalies post-click.
### Phishing Email Details:

| **Attribute**     | **Value**                                          |
|-------------------|----------------------------------------------------|
| Sender            | notifications@securebnk-support[.]com              |
| Subject           | Urgent: Update Your SecureBank Credentials         |
| Malicious Link    | http://securebnk-support[.]com/login               |
| Attack Vector     | Credential harvesting                              |

<br><br>

## 3. Impact Assessment

| **Factor**                | **Details**                                                                                  |
|---------------------------|----------------------------------------------------------------------------------------------|
| Affected User(s)          | 1 confirmed user (HR department)                                                             |
| Compromised Accounts      | 1 Office 365 email account (temporary access by attacker)                                    |
| Sensitive Data Accessed   | No customer data accessed, internal HR portal only                                           |
| Duration of Access        | 21 minutes before forced logout + password reset                                             |
| Business Impact           | Low (No transactions or data manipulation occurred)                                          |
| Regulatory Impact         | Reported to data protection authority under GDPR Article 33                                  |

<br><br>

## 4. Containment Actions

| **Time**   | **Action Taken**                                                                                               |
|------------|-----------------------------------------------------------------------------------------------------------------|
| 09:52 AM   | User account force-logged out from all sessions via Azure Security Center                                       |
| 09:54 AM   | Password reset enforced                                                                                         |
| 09:55 AM   | Multi-Factor Authentication (MFA) token invalidated and re-enrolled                                             |
| 10:00 AM   | Blocked phishing domain and IP in email gateway, firewall, and DNS filters                                      |
| 10:05 AM   | Notified all staff via secure messaging to beware of similar emails                                             |

<br><br>

## 5. Eradication and Recovery
### Actions Taken:
+ Disabled phishing site by notifying registrar and ISP
+ Conducted mailbox audit (forwarding rules, read messages)
+ Performed endpoint scan on user workstation (no malware detected)
+ Validated that no email rules or lateral movement occurred
+ Reviewed audit logs from Azure AD and internal apps
### Recovery Status:
+ User access restored after re-validation (11:15 AM)
+ All systems normal and monitored for 48 hours post-incident

<br><br>

## 6. Root Cause Analysis

| **Item**              | **Detail**                                                                                         |
|------------------------|----------------------------------------------------------------------------------------------------|
| Root Cause             | User clicked on a link in a well-crafted phishing email impersonating IT                           |
| Security Gap           | Email evaded basic filters; user failed to verify sender domain                                    |
| Contributing Factor    | User had not recently completed phishing awareness training                                        |

<br><br>

## 7. Lessons Learned

| **Area**             | **Finding**                                              | **Action Taken**                                 |
|----------------------|----------------------------------------------------------|--------------------------------------------------|
| Email Security       | Phishing email bypassed SPF/DKIM checks                 | Enhanced email filtering & DMARC policy          |
| User Awareness       | Target user lacked recent training                      | Mandatory re-training scheduled                  |
| Logging & Alerting   | SIEM detection effective, but slightly delayed          | Improve ingestion latency                        |
| Incident Playbooks   | Existing playbook followed effectively                  | Minor improvements noted for future              |

<br><br>

## 8. Recommended Improvements
### Immediate:
+ Conduct full phishing simulation for all departments
+ Update Office 365 mail flow rules to block lookalike domains
+ Strengthen automated email scanning policies with sandbox analysis
### Long-Term:
+ Quarterly phishing simulation & awareness training
+ Implement behavioral anomaly detection for high-risk accounts
+ Automate incident ticket generation from email alerts (SOAR integration)

<br><br>

## 9. Reporting and Compliance
### Regulatory Notification:
+ **GDPR Article 33** notification submitted to local Data Protection Authority (within 72 hours)
+ Incident documented in internal breach register
### Stakeholders Notified:
+ CISO
+ Legal and Compliance Team
+ Internal Audit
+ Data Protection Officer
### Final Documentation:
+ Executive Report (PDF)
+ Incident Timeline & Log Correlation (CSV)
+ IR Case File stored in company’s SIEM & IR documentation system


<br><br>


# Incident Response Report: Data Breach at Government Hospital
### Date of Incident: May 9, 2024
### Analyst: Hacker9 (Cybersecurity Analyst)
### Organization: SecureIT Cybersecurity Services (Client: Government Hospital)

<br><br>

## 1. Executive Summary

On May 9, 2024, SecureIT received an urgent notification from Government Hospital's internal security team regarding a data breach involving the unauthorized access of sensitive patient data. The breach was detected through anomalous access patterns in the hospital's Electronic Health Record (EHR) system. Initial investigation revealed that an internal user account was compromised, potentially exposing personal health information (PHI) of over 5000 patients.

SecureIT's Incident Response Team immediately took containment, investigation, and mitigation actions. The root cause was traced back to phishing and poor password hygiene. The breach was contained, and recovery efforts were initiated.

The hospital is working with regulatory authorities (such as HHS OCR under HIPAA) to report and mitigate the breach.

<br><br>

## 2. Incident Identification
### Detection Time:
+ May 9, 2024 — 11:15 AM
### How It Was Detected:
+ SIEM (Splunk) alert triggered by unusual login activities on hospital EHR system, with multiple failed login attempts followed by a successful login.
+ User behavior analysis from Identity & Access Management (IAM) logs identified an unusual login from an off-site IP address.
+ Security team member noticed a discrepancy in user access privileges and alerted the IT department.
### Indicators of Compromise (IOCs):
+ Suspicious IP from a foreign country accessing the EHR system.
+ Increased access rights on an administrative account that had been inactive for several months.
+ Multiple failed login attempts followed by a successful login.
+ Unusual file access — 5,000+ records accessed from the compromised account in an hour.

<br><br>

## 3. Impact Assessment

| **Category**             | **Details**                                                                                                                                                                                                                                                                                    |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compromised Account      | 1 internal user account in HR department                                                                                                                                                                                                                                                      |
| Type of Data Breached    | Personal Health Information (PHI), including names, addresses, medical histories, lab results, diagnoses, and treatment plans.                                                                                                                                                               |
| Total Records Exposed    | Approx. 5,000 patient records                                                                                                                                                                                                                                                                  |
| Duration of Breach       | Data access was occurring from 10:50 AM to 11:10 AM before account was locked and password reset.                                                                                                                                                                                             |
| Business Impact          | - Exposure of sensitive health information.<br>- Reputational damage.<br>- Potential legal and financial repercussions due to HIPAA non-compliance.                                                                                                                                          |
| Regulatory Impact        | - Notification of breach to HHS OCR (HIPAA) within 60 days.<br>- Potential fines for violation of data protection laws.                                                                                                                                                                       |
| Systems Affected         | - Electronic Health Records (EHR) system<br>- Internal user access systems and IAM<br>- Hospital’s internal network and email system.                                                                                                                                                         |

<br><br>

## 4. Containment Actions
### Immediate Steps Taken:

| **Time**   | **Action Taken**                                                                                                                                                        |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 11:20 AM   | Incident Response declared a Severity 1 breach and began containment procedures.                                                                                        |
| 11:25 AM   | Compromised user account disabled, and the associated password reset and MFA enforced.                                                                                  |
| 11:30 AM   | The SIEM system configured to increase sensitivity on suspicious logins and file access on other user accounts.                                                         |
| 11:35 AM   | Blocked the suspicious IP address from all hospital internal systems via the firewall.                                                                                  |
| 11:40 AM   | Conducted network segmentation to isolate systems believed to be affected by the breach and prevent lateral movement within hospital infrastructure.                   |
| 11:50 AM   | Notified hospital leadership, CISO, and legal team about the breach for further regulatory reporting.                                                                   |

<br><br>

## 5. Eradication and Recovery
### Actions Taken to Eliminate the Threat:

| **Time**      | **Action Taken**                                                                                                                                                                         |
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 12:00 PM      | Password reset for all internal user accounts with access to the EHR system.                                                                                                             |
| 12:15 PM      | Full endpoint scans conducted on the compromised workstation for signs of additional malware or backdoors.                                                                              |
| 12:30 PM      | Review and update security configurations on the EHR system to ensure least privilege access and prevent unauthorized access.                                                           |
| 1:00 PM       | Conducted a data integrity check to ensure no alterations or tampering occurred with the PHI accessed.                                                                                  |
| 1:30 PM       | Restore backups of any impacted files to ensure the system is fully secure and intact.                                                                                                  |
| 2:00 PM       | Engaged third-party forensic experts to perform a deeper analysis of any potential APT or other external intrusion tactics.                                                             |

### Recovery Status:
+ Systems: Fully restored and functional by 4:00 PM.
+ Monitoring: Enhanced monitoring in place for 48 hours post-incident to track any abnormal behavior.
+ Regulatory Compliance: Notified the hospital’s legal team to prepare for reporting under HIPAA breach notification requirements.

<br><br>

## 6. Root Cause Analysis

| **Root Cause**            | **Detail**                                                                                                                                                                           |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Initial Vector            | The breach was initiated via phishing — the attacker gained access by targeting an internal HR staff member. The attacker spoofed the internal hospital domain.                     |
| Exploited Vulnerabilities | The attacker exploited weak password hygiene and lack of MFA on an internal user account to gain unauthorized access.                                                              |
| Contributing Factors      | Lack of security awareness training on phishing for HR staff<br>Weak password policies for internal users.                                                                          |

<br><br>

## 7. Lessons Learned

| **Area**            | **Finding**                                                                                         | **Action Taken**                                                                                     |
|---------------------|------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Employee Awareness  | Phishing awareness was insufficient. The HR department was targeted.                                 | Immediate mandatory phishing awareness training for all staff.                                        |
| Access Controls     | Weak password policies and lack of multi-factor authentication (MFA) on HR accounts.                 | Enforced stronger password policies and mandatory MFA for all internal accounts.                      |
| Data Monitoring     | SIEM alerts were triggered but response time was not fast enough.                                    | Reduced alert threshold for suspicious activity in SIEM.                                              |
| Security Controls   | Lack of immediate network segmentation after suspicious behavior was identified.                     | Implemented network segmentation protocols for quicker containment.                                   |

<br><br>

## 8. Recommended Improvements
### Short-Term (Next 7 Days):
+ Enforce multi-factor authentication across all sensitive systems.
+ Implement advanced email filtering to catch spear-phishing emails.
+ Update password policies to ensure stronger passwords and regular rotation.
### Long-Term (Next 3 Months):
+ Quarterly phishing simulations for all hospital departments.
+ Conduct a full security audit to identify potential gaps in the hospital's security posture.
+ Implement an Incident Response Automation system (SOAR) to accelerate initial detection and response.

<br><br>

## 9. Reporting and Compliance
### Regulatory Notification:
+ HIPAA Breach Notification (to HHS OCR) submitted within the 60-day required timeframe.
+ Internal Reporting completed to ensure compliance with data protection laws.
+ Local Law Enforcement notified about the breach per internal policy for healthcare data breaches.
### Final Documentation:
+ Executive Summary Report (PDF)
+ Full Incident Timeline (CSV)
+ Case Investigation and Evidence Logs
### Supporting Documents
+ Phishing email samples and analysis.
+ Log review and audit report from SIEM.
+ Screenshots from EHR system with accessed records.
+ Endpoint investigation reports (from antivirus and malware scanners).
