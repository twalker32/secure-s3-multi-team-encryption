# 🔐 Secure S3 Multi-Team Encryption Architecture

This project demonstrates a secure, scalable architecture that enables multiple teams to share a single S3 bucket with **data segmentation**, **custom KMS key rotation**, and **auditable access logging**. It highlights encryption best practices using **AWS KMS Customer Managed Keys (CMKs)**, role-based access control, and CloudTrail-based visibility per team.

> 🧠 Use Case: A compliance-driven organization requires Team A, B, and C to store data in the same S3 bucket while maintaining encryption isolation, independent key rotation policies (30, 60, 90 days), and visibility into who accessed what.

---

## 📌 Key Features

- 🪪 **Customer Managed CMKs per team** with tightly scoped key policies
- 📂 **Logical data partitioning** via S3 prefix strategy
- 🔁 **Manual key rotation logic** (30/60/90-day cycles) using alias redirection
- 📜 **IAM Policies** to enforce CMK-per-team usage
- 👁️ **CloudTrail integration** for key usage and data access auditing
- 🧪 Ready-to-use CLI scripts for CMK creation, S3 encryption, and alias updates

---

## 🧱 Architecture Overview

![Architecture Diagram](architecture-diagram.png)

- Single S3 bucket with prefixes:
  - `/team-a/`, `/team-b/`, `/team-c/`
- Each team is assigned its own CMK: `cmk-team-a`, `cmk-team-b`, `cmk-team-c`
- Object uploads must use the corresponding CMK
- IAM and bucket policies restrict access to relevant prefixes + CMKs
- Logging via CloudTrail and S3 Data Events for traceability

---

## 📂 Repository Contents

| File / Folder | Description |
|---------------|-------------|
| `architecture-diagram.png` | Visual of the overall architecture |
| `s3-encryption-setup.md` | Step-by-step guide to configure the S3 bucket and CMKs |
| `key-rotation-guide.md` | Strategy and script for rotating keys every 30/60/90 days |
| `cloudtrail-logs-sample.md` | Sample CloudTrail queries to audit data access per team |
| `iam-policies/` | JSON files for fine-grained IAM and key access policies |
| `scripts/` | Automation scripts for CMK creation, alias rotation, and encryption |

---

## 🚀 How to Use

1. Clone the repo
2. Follow the setup instructions in `s3-encryption-setup.md`
3. Use the provided IAM policies and scripts for CMK enforcement and key alias management
4. Enable CloudTrail and S3 Data Events for full audit coverage
5. Rotate keys as needed using the guide

---

## 🎯 Skills Demonstrated

- AWS KMS & Envelope Encryption
- IAM Policy Design
- Data Isolation in Shared Buckets
- Manual Key Lifecycle Management
- CloudTrail Logging & Compliance Visibility
- Secure Architecture Design Thinking

---

## 🧠 Lessons Learned

This project deepened my understanding of:
- CMK vs AWS-managed keys
- CMK alias rotation as a workaround for sub-365-day key rotation
- Using CloudTrail to trace encryption and data access per business unit
- Building secure multi-tenant data environments on AWS

---

## 📬 Contact

> **Timothy Walker**  
Cloud Security Architect Analyst  
[LinkedIn](https://www.linkedin.com/in/your-link) | [GitHub](https://github.com/twalker32)

---

_This project is part of the Tech Talks with Tim (T³) portfolio, where I share hands-on architecture patterns for secure, governed, cloud-native infrastructure._

