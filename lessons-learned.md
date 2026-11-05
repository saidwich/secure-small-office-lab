# Lessons Learned

Problems I ran into while building this lab, how I diagnosed them, and what I took away.

---

## 1. Locked out of my own server
**Problem:** "Login incorrect" on the Ubuntu server console right after installing it.
**Diagnosis:** Ubuntu shows nothing while you type a password, so I couldn't tell whether I was mistyping. Usernames are also case-sensitive.
**Fix:** Retyped every command carefully
**Takeaway:** Record credentials when you create an account, and learn the recovery process before you need it.

## 2. `chmod` command failed: `sudo: '2770': command not found`
**Problem:** A permissions command was split across lines in the console, so Linux tried to run `2770` as a program.
**Fix:** Retyped the command as one line starting with `sudo chmod`, then confirmed with `ls -l`.
**Takeaway:** Read the error message literally. Here it said exactly what happened. I also learned the VirtualBox console handles pasted text poorly, so I did more work over SSH.

## 3. Running commands on the wrong machine
**Problem:** `nmap: command not found`, and later fail2ban refused to ban anything.
**Diagnosis:** My prompt said `sysadmin@srv1`, so I was running attacker-side commands on the server. fail2ban's own log said it was ignoring the address "by ignoreself rule" because I was SSHing from the server to itself.
**Fix:** Ran scans and attack tests from Kali, and server configuration on srv1.
**Takeaway:** Check the prompt and hostname before running anything. Two similar-looking terminals are easy to mix up, and a security test only counts if it comes from the right place.

## 4. fail2ban saw nothing
**Problem:** fail2ban showed 0 failed logins even though the logs clearly recorded them.
**Diagnosis:** Newer Ubuntu logs SSH to the systemd journal instead of `auth.log`, and newer OpenSSH logs under the `sshd-session` process. fail2ban's default journal filter matched `sshd` only.
**Fix:** Set `backend = systemd` and overrode `journalmatch` to match the service unit instead.
**Takeaway:** When a tool reports nothing, compare what it is looking at with where the data really is.

## 5. SSH wouldn't restart after a config change
**Problem:** `ssh.service failed` after I edited my hardening file.
**Diagnosis:** `sshd -t` pointed to line 2: I had typed `Password Authentication` with a space instead of `PasswordAuthentication`.
**Fix:** Rewrote the file, confirmed `sshd -t` printed nothing, then restarted.
**Takeaway:** Always test a config before restarting a critical service, and keep a console session open so a mistake can't lock me out.

## 6. A setting I changed had no effect
**Problem:** My file said `PasswordAuthentication no`, but `sshd -T` still showed `yes`.
**Diagnosis:** SSH uses the first value it reads, and `50-cloud-init.conf` loaded before my `99-` file and set `yes`.
**Fix:** Renamed my file to `00-hardening.conf` so it loads first, and verified with `sshd -T`.
**Takeaway:** Check the effective configuration, not just the file you edited.

## 7. Guest ACL design
**Observation:** The Guest ACL needed explicit permits for DHCP and DNS before the deny rules, or Guest devices couldn't get an address or resolve names. ACLs are processed top to bottom, so rule order is the design.
**Limitation:** The ACL is stateless, so [Staff cannot ping Guest because replies are blocked / delete this line if you didn't observe it]. A stateful firewall would handle this better.

---

## What I would do next
- Add central logging and alerting (for example Wazuh) so attacks are detected, not just blocked.
- Replace the Packet Tracer router and ACL with a stateful firewall such as pfSense.
- Add Active Directory and Group Policy for centralized user management.
