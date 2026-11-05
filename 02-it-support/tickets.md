# Support Tickets

Tickets below are based on issues I reproduced and resolved in my lab environment
(Ubuntu Server + Samba, Kali client, and the Packet Tracer office network).

---

### Ticket #001 | Priority: Medium | Category: Account
**Reported by:** Alice (Staff)
**Issue:** User forgot her password and cannot log in.
**Troubleshooting:** Confirmed the username exists (`id alice`) and verified the request was from the account owner.
**Resolution:** Reset the password with `passwd alice` and forced a change at next login with `chage -d 0 alice`.
**Time to resolve:** 5 min
**Prevention:** Encourage a password manager and document the self-service reset process.

---

### Ticket #002 | Priority: Medium | Category: Account
**Reported by:** Bob (Staff)
**Issue:** User cannot log in even though the password is correct.
**Troubleshooting:** Checked the account status with `passwd -S bob` and found it was locked (`L`).
**Resolution:** Unlocked the account with `passwd -u bob`, confirmed login works, and advised the user to contact IT if it happens again.
**Time to resolve:** 5 min
**Prevention:** Review the lock policy and check logs for repeated failed attempts before unlocking.

---

### Ticket #003 | Priority: High | Category: Network
**Reported by:** Staff-PC2 user
**Issue:** "Internet is down." Websites and file server names do not open.
**Troubleshooting:** `ipconfig` showed a valid IP, and `ping 192.168.20.10` worked, but `ping fileserver.office.local` failed. This pointed to DNS. `ipconfig /all` showed the wrong DNS server (192.168.20.99).
**Resolution:** Switched the PC back to DHCP so it received the correct DNS server (192.168.20.10). Name resolution worked again.
**Time to resolve:** 10 min
**Prevention:** Avoid manual DNS settings on user PCs and keep DHCP as the default.

---

### Ticket #004 | Priority: Medium | Category: Performance
**Reported by:** Staff user
**Issue:** Computer is very slow.
**Troubleshooting:** Ran `top` and found one process using ~100% CPU.
**Resolution:** Identified the runaway process by PID, stopped it with `kill`, and confirmed CPU usage returned to normal.
**Time to resolve:** 10 min
**Prevention:** Monitor resource usage and investigate what started the process.
