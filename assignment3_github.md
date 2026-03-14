# Assignment 3: Attack and Defend Web Application

![Security](https://img.shields.io/badge/Security-Web%20Application-red)
![WAF](https://img.shields.io/badge/WAF-ModSecurity%20%2B%20OWASP%20CRS-blue)
![DVWA](https://img.shields.io/badge/Target-DVWA-orange)
![PrestaShop](https://img.shields.io/badge/Target-PrestaShop%209.0.3-purple)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

## Overview

This project demonstrates practical web application attack and defence techniques using industry-standard tools. It covers SQL Injection and XSS attacks on DVWA, ModSecurity WAF configuration, and a full security audit and attack simulation on a live PrestaShop deployment.

---

## Objectives

- Execute SQL Injection and XSS attacks on DVWA
- Configure ModSecurity WAF with OWASP Core Rule Set to block attacks
- Re-test and validate WAF effectiveness
- Deploy PrestaShop and complete a full security checklist
- Simulate attacks on PrestaShop and document findings

---

## Lab Environment

| Component | Detail |
|-----------|--------|
| Attacker | Kali Linux (172.20.10.9) |
| Target | Ubuntu 24.04 (172.20.10.7) |
| Web Server | Apache 2.4.58 |
| PHP | 8.3.6 |
| Database | MariaDB 10.11.14 |
| WAF | ModSecurity 2.9.7 + OWASP CRS 3.3.5 |
| DVWA | Low security level |
| PrestaShop | Version 9.0.3 |

---

## Part 1 — SQL Injection on DVWA

Attacks were performed on the DVWA SQL Injection module with security set to Low.

| Payload | Result |
|---------|--------|
| `' OR '1'='1` | All user records dumped |
| `' UNION SELECT null,version()#` | Database version exposed |
| `' UNION SELECT null,database()#` | Database name exposed |
| `' UNION SELECT null,table_name FROM information_schema.tables WHERE table_schema=database()#` | All table names dumped |

---

## Part 2 — XSS on DVWA

| Type | Payload | Result |
|------|---------|--------|
| Reflected XSS | `<script>alert('XSS')</script>` | Alert popup fired |
| Stored XSS | `<script>alert('Stored XSS')</script>` | Persistent popup on every page load |

---

## Part 3 — ModSecurity WAF Configuration

### Installation

```bash
sudo apt install libapache2-mod-security2 -y
sudo a2enmod security2
sudo systemctl restart apache2
```

### Enable Enforcement Mode

```bash
sudo cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
```

Edit `/etc/modsecurity/modsecurity.conf`:

```
SecRuleEngine On
```

### OWASP CRS Loading

Configured in `/etc/apache2/mods-enabled/security2.conf`:

```apache
IncludeOptional /etc/modsecurity/*.conf
IncludeOptional /usr/share/modsecurity-crs/*.load
```

### WAF Validation

Log entry from `/var/log/apache2/modsec_audit.log`:

```
Action: Intercepted (phase 2)
Engine-Mode: "ENABLED"
Producer: ModSecurity for Apache/2.9.7
OWASP_CRS/3.3.5
```

### WAF Re-test Results

| Attack | Before WAF | After WAF |
|--------|-----------|-----------|
| SQL Injection | Users dumped | 403 Forbidden |
| Reflected XSS | Alert fired | 403 Forbidden |
| Stored XSS | Persistent popup | 403 Forbidden |

---

## Part 4A — PrestaShop Security Checklist

### Security Checklist Results

| # | Item | Status | Action Taken |
|---|------|--------|--------------|
| 1 | Application and modules up to date | ✅ Pass | 4 modules updated, memory_limit increased to 256M |
| 2 | Remove default/demo accounts and data | ✅ Pass | 2 demo customers, 19 products, 4 carriers deleted |
| 3 | Strong admin password and MFA | ⚠️ Partial | Password policy hardened, MFA requires third-party module |
| 4 | File/folder permissions and directory listing | ✅ Pass | Options -Indexes added, permissions verified at 755/644 |
| 5 | Secure session cookie settings | ✅ Pass | SameSite=Strict, HttpOnly confirmed, cookie lifetimes configured |
| 6 | Disable unnecessary modules | ✅ Pass | Payment and unused modules disabled |
| 7 | Payment and API endpoint configuration | ✅ Pass | Webservice disabled, API returns 401 |
| 8 | TLS configured and enforced | ⚠️ Fail | HTTP only in lab — security headers added as mitigation |
| 9 | Exposed admin paths and IP restrictions | ✅ Pass | Admin path randomized, IP restricted to 172.20.10.0/28 |
| 10 | Backup and recovery | ✅ Pass | Database and files backup created and verified |

**Result: 8/10 Pass | 1/10 Partial | 1/10 Fail (lab limitation)**

---

## Part 4B — PrestaShop Attack Simulation

### Attack 1 — Directory Enumeration

```bash
dirb http://172.20.10.7 /usr/share/dirb/wordlists/common.txt
```

| Finding | Response | Risk |
|---------|----------|------|
| /api | 401 Unauthorized | API endpoint exposed |
| /Makefile | 200 OK | Sensitive file accessible |
| /login | 200 OK | Login page enumerated |
| /upload | Directory found | Upload path exposed |
| /config, /vendor, /src | 403 Forbidden | Protected correctly |

- **Without WAF:** 54 paths discovered
- **With WAF:** All requests blocked — 403 Forbidden
- **Risk:** Medium

---

### Attack 2 — XSS via Search Bar

```
http://172.20.10.7/search?s=<script>alert('XSS')</script>
```

- **Without WAF:** Mitigated by PrestaShop native output escaping
- **With WAF:** 403 Forbidden
- **Risk:** Low

---

### Attack 3 — SQL Injection via Search

```
http://172.20.10.7/search?s='+OR+'1'='1
```

- **Without WAF:** Mitigated by Doctrine ORM prepared statements
- **With WAF:** 403 Forbidden
- **Risk:** Low

---

### Attack 4 — Admin Panel Brute Force

```bash
hydra -l admin@testshop.com -P /usr/share/wordlists/rockyou.txt 172.20.10.7 \
http-post-form "/admin740oaqd6tpzv7pcpuno/index.php:email=^USER^&passwd=^PASS^&submitLogin=1:Invalid" \
-V -f -t 4
```

- **Without WAF:** 880+ attempts made with no lockout triggered
- **With WAF:** 403 Forbidden — blocked immediately
- **Risk:** High

---

## Key Findings and Recommendations

| # | Finding | Priority |
|---|---------|----------|
| 1 | No account lockout on admin login | High — enable rate limiting |
| 2 | No TLS in lab environment | High — required for production |
| 3 | MFA not available natively | Medium — use third-party module |
| 4 | Makefile publicly accessible | Medium — permissions hardened |
| 5 | 54 paths enumerable without WAF | Medium — WAF and Indexes mitigated |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| DVWA | Vulnerable web application target |
| ModSecurity | Web Application Firewall |
| OWASP CRS 3.3.5 | WAF rule set |
| dirb | Directory enumeration |
| Hydra 9.5 | Brute force simulation |
| Firefox | Manual attack testing |
| curl | API and header testing |
| mysqldump | Database backup |

---

## References

- [ModSecurity Documentation](https://www.modsecurity.org/)
- [OWASP Core Rule Set](https://coreruleset.org/)
- [DVWA Project](https://dvwa.co.uk/)
- [PrestaShop Security Guidelines](https://devdocs.prestashop-project.org/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
