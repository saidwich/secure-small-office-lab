# Security Assessment and Hardening Report

**Target:** srv1 (Ubuntu Server, Samba file server)
**Scanner:** Kali Linux with Nmap
**Scope:** My own lab environment only

## Method
1. Scanned srv1 before changes (`nmap -sV -p-`)
2. Documented findings and risks
3. Applied hardening
4. Re-scanned to verify the fixes

## Findings and Fixes

| # | Finding | Risk | Fix Applied | Verified By |
|---|---|---|---|---|
| 1 | No host firewall | All listening ports reachable | UFW: default deny, allow SSH and SMB from lab subnet only | `ufw status`, re-scan |
| 2 | Apache web server running (staged test service) | Unnecessary attack surface | Removed Apache | Port 80 no longer open |
| 3 | SSH password authentication enabled | Brute-force exposure | Key-based login only, root login disabled | `ssh-auth-methods` lists only publickey |
| 4 | No limit on failed SSH logins | Unlimited password guessing | `MaxAuthTries 3`, fail2ban (3 failures = 10 min ban) | Ban confirmed in `fail2ban-client status` |
| 5 | Samba open to any source | Wider exposure | Firewall restricts SMB to 192.168.56.0/24, minimum protocol SMB2 | Re-scan, share still works |
| 6 | Pending package updates | Known vulnerabilities | `apt upgrade` | `apt list --upgradable` is empty |

## Before and After

| Metric | Before | After |
|---|---|---|
| Open TCP ports | [your number] | [your number] |
| SSH password login | Enabled | Disabled |
| SSH root login | [your value] | Disabled |
| Host firewall | None | UFW active |
| Brute-force protection | None | fail2ban active |

## Evidence
- `before-scan.txt` / `after-scan.txt`
- Screenshots in `/screenshots`

## Limitations and Next Steps
- Apache was staged to simulate a forgotten service; a fresh Ubuntu install has few exposed services.
- The scan was against a single host on a host-only network.
