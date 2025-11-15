# Deployment Steps – Cross Account S3 Access

This guide shows how to enable cross-account access to an S3 bucket using an IAM role.

---

### 1. Dev Account – Create IAM User
1. IAM → Create user → Name: `Alice`  
2. Provide AWS Management Console access  
3. Set custom password and uncheck “User must create a new password at next sign-in”  
4. Copy credentials:
   - Console sign-in URL: `https://accountid.signin.aws.amazon.com/console`  
   - User: `Alice`  
   - Password: *********

### 2. Prod Account – Create S3 Bucket and IAM Role
1. S3 → Create bucket → Name: `bucket-14-11-25`  
2. Upload an object to the bucket  
3. IAM → Roles → Create role  
   - Trusted entity: AWS Account → Another AWS Account → Dev Account ID  
   - Permissions: AmazonS3FullAccess  
   - Role name: `crossaccounts3access`

### 3. Dev Account – Attach Inline Policy to Alice
JSON Example:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::PROD_ACCOUNT_ID:role/crossaccounts3access"
    }
  ]
}

Or via Service: STS → All actions (sts:*) → Resources: All → Policy name: stspolicy

### 4. Switch Role and Access Bucket
Log in as Alice (Incognito)
Switch role: Prod Account ID → Role: crossaccounts3access → Display name: crossaccounts3access@123
Access bucket and objects successfully

