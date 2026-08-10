# Project 1 — SSH Honeypot & Network Attack Detection

## Overview

A controlled network-security lab focused on SSH reconnaissance, simulated brute-force activity, honeypot monitoring, and incident reporting.

## Lab Environment

| System | Role | IP |
|---|---|---|
| Kali Linux | Attacker / reconnaissance | 172.20.10.9 |
| Ubuntu + Cowrie | SSH honeypot / target | 172.20.10.7 |

## Objectives

- Deploy an SSH honeypot using Cowrie.
- Perform reconnaissance against the lab target.
- Simulate SSH attack activity from Kali Linux.
- Capture and analyze network traffic.
- Document the incident and identify defensive controls.

## Attack Surface

The primary protocol involved was SSH over TCP port 22.

## Attack Flow

1. Cowrie was deployed on the Ubuntu target.
2. Kali Linux performed reconnaissance against the target.
3. SSH attack activity was generated against the honeypot.
4. Network traffic was captured for analysis.
5. Honeypot activity and network evidence were reviewed.
6. Findings were documented in an incident report.
7. Fail2Ban and SSH rate limiting were identified as remediation measures.

## Evidence

Evidence associated with the lab includes:

- Cowrie running on the Ubuntu host
- Nmap reconnaissance results
- Wireshark network capture
- Honeypot/attack logs
- Incident report

Sensitive or identifying information should be removed from screenshots and captures before public release.

## Security Lessons

This lab demonstrated how exposed SSH services can become targets for reconnaissance and brute-force activity, and how honeypots and network monitoring can provide useful detection evidence.

## Remediation

A recommended control is **Fail2Ban with SSH rate limiting** to detect repeated authentication failures and temporarily block abusive sources.

## Scope

All activity was conducted against intentionally configured lab systems in a controlled environment.
