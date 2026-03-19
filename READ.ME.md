
 Cloud Privilege Escalation Simulation

**Platform:** Amazon Web Services (AWS)  
**Tools:** AWS CLI v2 | Kali Linux (aarch64)  
**Focus:** IAM Misconfiguration | Privilege Escalation | Least Privilege Remediation



 Objective

Demonstrate how IAM misconfigurations in AWS can lead to full privilege escalation 
and remediate the vulnerability using the Principle of Least Privilege.



 Task 1 - Create Misconfigured IAM User

Created an IAM user and attached the `AdministratorAccess` managed policy, 
granting unrestricted access to all AWS services. Access keys were generated 
to simulate compromised credentials.

bash
aws iam create-user --user-name misconfigured-user
aws iam attach-user-policy \
  --user-name misconfigured-user \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

<img width="1680" height="1050" alt="iamfinal" src="https://github.com/user-attachments/assets/c23e9e3c-fee4-4af0-bf54-dbedfba44e9e" />
> Screenshot: User creation and policy attachment

 Task 2 - Privilege Escalation Attempt

Using the misconfigured-user credentials, the following actions were performed 
to simulate an attacker with stolen keys:

1. Confirmed identity using `aws sts get-caller-identity`
2. Enumerated all IAM users using `aws iam list-users`
3. Created a backdoor account `attacker-user`
4. Granted `attacker-user` full AdministratorAccess for persistent access
5. Confirmed full access to S3 and EC2 infrastructure

bash
aws iam create-user --user-name attacker-user --profile misconfigured
aws iam attach-user-policy \
  --user-name attacker-user \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess \
  --profile misconfigured
aws s3 ls --profile misconfigured
aws ec2 describe-instances --profile misconfigured


**Result:** Full privilege escalation achieved. One compromised key resulted 
in a persistent backdoor admin account with unrestricted AWS access.

> Screenshot: attacker-user creation and S3/EC2 access confirmed

<img width="1680" height="1050" alt="iamweb" src="https://github.com/user-attachments/assets/df9547b5-c640-426d-83bd-e2ded1cea4be" />


 Task 3 - Fix Using Principle of Least Privilege

Removed `AdministratorAccess` and replaced it with a custom policy granting 
only S3 read access.

bash
aws iam detach-user-policy \
  --user-name misconfigured-user \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

aws iam create-policy \
  --policy-name least-privilege-policy \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": ["s3:GetObject", "s3:ListBucket"],
        "Resource": "*"
      }
    ]
  }'


**Escalation test after fix:**
bash
aws iam create-user --user-name test-blocked --profile misconfigured
 Result: AccessDenied - escalation blocked


> Screenshot: AccessDenied confirming escalation is blocked
<img width="1680" height="1050" alt="iamfinal" src="https://github.com/user-attachments/assets/0e0176d1-ec61-43e4-b03a-d4de36f117e3" />



Vulnerability and Resolution

 What Went Wrong
Attaching `AdministratorAccess` to a regular IAM user grants wildcard permissions 
across all AWS services. An attacker with these credentials can enumerate accounts, 
create backdoors, access data, and persist even after keys are revoked.

MITRE ATT&CK Mapping

| Technique | ID | Description |

| Valid Accounts | T1078 | Using compromised IAM credentials |
| Cloud Account Creation | T1136.003 | Creating backdoor attacker account |
| Account Manipulation | T1098 | Granting attacker-user admin access |

Resolution
Applied Principle of Least Privilege — restricted user to S3 read-only access. 
All IAM actions are now denied. Backdoor account deleted during cleanup.

 Best Practices
- Never assign `AdministratorAccess` to regular IAM users
- Use IAM roles instead of long-lived access keys
- Enable MFA on all IAM users
- Enable AWS CloudTrail to log all API activity
- Audit policies regularly using AWS IAM Access Analyzer



 Disclaimer
All actions were performed in a controlled AWS environment for educational purposes only.


