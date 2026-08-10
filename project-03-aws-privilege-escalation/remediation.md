# AWS IAM Remediation

## Root Cause

The primary misconfiguration was assigning `AdministratorAccess` to a regular IAM user.

## Remediation

Replace broad administrative permissions with only the permissions required for the user's legitimate task.

For this simulation, the user was restricted to S3 read-only operations:

- `s3:GetObject`
- `s3:ListBucket`

## Verification

An attempt to create another IAM user after remediation returned:

`AccessDenied`

This confirmed that the user could no longer perform IAM account-creation actions.

## Additional Controls

- Use IAM roles where possible.
- Avoid unnecessary long-lived access keys.
- Enable MFA.
- Monitor API activity with CloudTrail.
- Review IAM policies regularly.
- Use IAM Access Analyzer to identify excessive permissions.
