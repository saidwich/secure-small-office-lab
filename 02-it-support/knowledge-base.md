# Knowledge Base

---

## KB-001: How to reset a user's password (Linux)

**When to use:** A user forgot their password or needs a reset.

1. Verify the user's identity (a manager approval or a ticket from their official email).
2. Confirm the account exists: `id <username>`
3. Set a temporary password: `sudo passwd <username>`
4. Force a change at next login: `sudo chage -d 0 <username>`
5. Give the temporary password through a secure channel, not in a public chat.
6. Ask the user to log in and confirm it works, then close the ticket.

---

## KB-002: How to troubleshoot "No internet"

**When to use:** A user says the internet or network is not working.

1. Ask what exactly fails: everything, one website, or one app.
2. Check the IP address: `ipconfig` (Windows) or `ip a` (Linux).
   - `169.254.x.x` means DHCP failed. Check the cable, switch port, VLAN, and DHCP server.
3. Ping the default gateway. A failure means a local network or VLAN problem.
4. Ping an outside IP address (e.g. `203.0.113.2`). A failure means a routing problem.
5. Ping a name (e.g. `ping fileserver.office.local`).
   - Works by IP but not by name means a **DNS problem**. Check the DNS server setting with `ipconfig /all`.
6. Fix the cause, test again, and document the result in the ticket.

---

## KB-003: How to fix "Access denied" on a shared folder

**When to use:** A user cannot open or save to a file share.

1. Confirm which share and which action fails (open, save, delete).
2. Check the user's groups: `id <username>`
3. Check the folder permissions: `ls -l /srv/shares`
4. Check the share's `valid users` line in `/etc/samba/smb.conf`.
5. If the user should have access, get approval, then add them: `sudo usermod -aG <group> <username>`
6. Ask the user to reconnect to the share (a new session is needed to pick up the group change).
7. If the user should **not** have access, explain why and close the ticket.
