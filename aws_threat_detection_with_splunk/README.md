# AWS Threat Detection using Splunk and MITRE ATT&CK

This project demonstrates how to detect various security threats in an AWS environment using CloudTrail logs analyzed in Splunk. It includes detection logic mapped to the MITRE ATT&CK framework and Splunk SPL queries for actionable threat detection.

---

## 📦 Files Included

- `sample_attack_logs.json` – Simulated AWS CloudTrail logs based on MITRE attacks.
- `spl_detection_queries.txt` – SPL queries to detect specific AWS-based threats.
- `README.md` – This guide.

---

## ✅ Step-by-Step Setup

### 1. 🔧 Set Up Splunk

- Install Splunk Enterprise (Free Trial or local setup): https://www.splunk.com/en_us/download/splunk-enterprise.html
- Launch Splunk and log in to the dashboard.

### 2. 📥 Ingest Logs into Splunk

- Go to **Settings > Add Data > Upload**
- Select the file `sample_attack_logs.json`
- Source type: `json`
- Index: Create new index called `aws_logs`
- Click **Review** and **Submit**

---

## 🔍 Step 3: Run SPL Queries for Detection

Open **Search & Reporting** app in Splunk and run the following queries:

#### ✅ Detect CreateAccessKey Activity (MITRE: T1078.004)
```spl
index=aws_logs eventName=CreateAccessKey
| eval mitre_technique="T1078.004 - Valid Accounts: Cloud Accounts"
```

#### ✅ Detect IAM Policy Changes (MITRE: T1098)
```spl
index=aws_logs eventName=PutUserPolicy OR eventName=CreateUser
| eval mitre_technique="T1098 - Account Manipulation"
```

#### ✅ Detect Brute Force on Console Login
```spl
index=aws_logs eventName=ConsoleLogin responseElements.ConsoleLogin="Failure"
| stats count by userIdentity.arn, sourceIPAddress
| where count > 5
```

---

## 📊 Step 4: Create Dashboards (Optional)

- Go to **Dashboard** app in Splunk
- Create panels for each detection logic
- Include severity, time, user, and IP address fields

---

## 📌 Use Cases Covered

| Use Case                                | MITRE Technique |
|----------------------------------------|------------------|
| CreateAccessKey misuse                 | T1078.004        |
| Excessive IAM policy updates           | T1098            |
| Failed login brute force               | T1110            |
| Unusual user enumeration               | T1087            |

---

## 💡 Tip

You can generate your own CloudTrail logs using AWS Free Tier and upload them manually to extend this project.

---

## 🔚 Outcome

This project simulates real-world AWS attacks and teaches you how to detect them using Splunk with MITRE mapping. Ideal for cybersecurity analyst portfolios.
