# Project 3 — AWS IAM Privilege Escalation Simulation

## Overview

A controlled AWS security exercise demonstrating how excessive IAM permissions can enable privilege escalation and persistent access, followed by remediation using the Principle of Least Privilege.

## Environment

- Kali Linux (aarch64)
- AWS CLI v2
- Amazon Web Services

## Objective

Demonstrate how an IAM misconfiguration can lead to privilege escalation and remediate the issue by applying least privilege.

## 1. Misconfigured IAM User

An IAM user named `misconfigured-user` was created and granted the AWS-managed `AdministratorAccess` policy.

This configuration provided unrestricted access across AWS services and was used to simulate compromised credentials.

## 2. Privilege Escalation Simulation

Using the simulated compromised credentials:

- Identity was verified with AWS STS.
- IAM users were enumerated.
- A second account, `attacker-user`, was created.
- `attacker-user` was granted `AdministratorAccess`.
- Access to S3 and EC2 was confirmed.

The simulation demonstrated how excessive permissions could allow an attacker to create persistent administrative access.

## 3. Remediation

`AdministratorAccess` was removed from `misconfigured-user` and replaced with a custom policy providing S3 read-only permissions:

- `s3:GetObject`
- `s3:ListBucket`

An escalation attempt using `iam:CreateUser` subsequently returned `AccessDenied`.

## Security Concepts

- Principle of Least Privilege
- Valid Accounts — MITRE ATT&CK T1078
- Cloud Account Creation — T1136.003
- Account Manipulation — T1098

## Best Practices

- Avoid assigning `AdministratorAccess` to regular users.
- Prefer IAM roles over long-lived access keys.
- Enable MFA.
- Enable CloudTrail.
- Review permissions regularly with IAM Access Analyzer.

## Cleanup

The simulated backdoor account was deleted during cleanup.

## Scope

The activity was performed as a controlled AWS security simulation.
