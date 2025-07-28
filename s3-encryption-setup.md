# 🛠️ S3 Encryption Setup Guide (Multi-Team CMK Enforcement)

This guide walks through how to:
- Create a shared S3 bucket
- Assign folder prefixes for each team
- Create Customer Managed CMKs for Team A, B, and C
- Configure IAM and bucket policies to enforce access
- Apply SSE-KMS with the correct CMKs
- Enable audit logging for compliance

---

## 🪣 Step 1: Create the S3 Bucket

```bash
aws s3api create-bucket --bucket your-org-secure-data --region us-east-1
```
### Example bucket structure
s3://your-org-secure-data/team-a/  
s3://your-org-secure-data/team-b/  
s3://your-org-secure-data/team-c/  

---

## 🪣 Step 2: Create 3 Customer Managed Keys (CMKs)

```bash
aws kms create-key --description "Team A Encryption Key"  
aws kms create-key --description "Team B Encryption Key"  
aws kms create-key --description "Team C Encryption Key"  
```
### 📌 Assign aliases:

```bash
aws kms create-alias --alias-name alias/team-a-key --target-key-id <KeyIdA>  
aws kms create-alias --alias-name alias/team-b-key --target-key-id <KeyIdB>  
aws kms create-alias --alias-name alias/team-c-key --target-key-id <KeyIdC>  
```
## 🛡️ Step 3: Attach CMK Key Policies
- Limit key usage to only the team’s IAM roles
- Deny usage by unauthorized principals

📂 Refer to: iam-policies/team-a-policy.json for examples
```json
"Condition": {
  "StringEquals": {
    "aws:PrincipalTag/Team": "TeamA"
  }
}
```
## 📜 Step 4: Define IAM + S3 Bucket Policies
Each team should:
- Access only their prefix
- Encrypt with only their CMK

```json
"Condition": {
  "StringEquals": {
    "s3:prefix": "team-a/*",
    "kms:EncryptionContext:aws:s3:arn": "arn:aws:s3:::your-org-secure-data/team-a/*"
  }
}
```
## 🔒 Step 5: Upload Objects with CMK Encryption

Use the appropriate CMK alias per team:
```bash
aws s3 cp confidential.txt s3://your-org-secure-data/team-a/ \
  --sse aws:kms --sse-kms-key-id alias/team-a-key
```
Repeat for teams B and C using their repsective aliases.

## 🔒 Step 6: Enable CloudTrail + S3 Data Events

Create the trail:
```bash
aws cloudtrail create-trail \
  --name org-access-trail \
  --s3-bucket-name your-trail-logs

aws cloudtrail start-logging --name org-access-trail
```

## ✅ Verification Checklist
- All CMKs created and aliased
- IAM + Key policies scoped per team
- Uploads succeed only when proper CMK is used
- CloudTrail and Data Events capture access

## 🧠 Pro Tip
Tag your CMKs to track rotation windows:

```bash
aws kms tag-resource --key-id <KeyId> \
  --tags TagKey=RotationCycle,TagValue=30Days
```
Use EventBridge + Lambda to automate key rotation alerts.

➡️ Ready for rotation? Head to key-rotation-guide.md  
➡️ Need IAM examples? See iam-policies/ folder  





