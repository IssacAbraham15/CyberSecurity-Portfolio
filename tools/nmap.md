# 🛠️ Nmap Cheat Sheet

**Nmap (Network Mapper)** is a powerful open-source tool for network discovery, port scanning, service enumeration, and vulnerability detection.

---

## 🔍 Basic Scans

| Command | Description |
|--------|-------------|
| `nmap <target>` | Default scan (1000 ports, no service detection) |
| `nmap -sP <network>/24` | Ping scan to discover live hosts |
| `nmap -sn <network>/24` | Host discovery without port scan (same as `-sP`) |

---

## 🚪 Port Scanning

| Command | Description |
|--------|-------------|
| `nmap -p 22,80,443 <target>` | Scan specific ports |
| `nmap -p- <target>` | Scan all 65,535 TCP ports |
| `nmap -F <target>` | Fast scan (100 common ports) |

---

## 🎯 Scan Techniques

| Option | Description |
|--------|-------------|
| `-sS` | TCP SYN scan (stealth scan - default if run as root) |
| `-sT` | TCP connect scan (used if not root) |
| `-sU` | UDP scan |
| `-sA` | ACK scan (used for firewall rule discovery) |
| `-Pn` | No ping (treat all hosts as online) |

---

## 🧠 Service & OS Detection

| Option | Description |
|--------|-------------|
| `-sV` | Service version detection |
| `-O` | Operating system detection |
| `--osscan-guess` | Attempt OS guess if standard detection fails |
| `-A` | Aggressive scan (includes `-sV`, `-O`, `--traceroute`, `--script=default`) |

---

## 📜 Output Formats

| Option | Description |
|--------|-------------|
| `-oN output.txt` | Normal output |
| `-oG output.gnmap` | Greppable output |
| `-oX output.xml` | XML output (for import into other tools) |
| `-oA <basename>` | Output in all 3 formats at once |

---

## 🧪 Useful Examples

```bash
nmap -sS -sV -p- -T4 <target>
nmap -A -T4 <target>
nmap -sn 192.168.1.0/24
nmap -sV -oA scans/myscan <target>
```

---

## 🕵️‍♂️ NSE (Nmap Scripting Engine)

```bash
nmap --script=vuln <target>
nmap --script smb-vuln* -p 445 <target>
```

---

## 🧠 Tips

- Use `-T4` or `-T5` for faster scans (but can be noisy)
- Combine with tools like `searchsploit` after version detection
- Run UDP scans separately — they're slow but valuable
- Use output files (`-oA`) to keep records for reports

---

📁 [Back to Portfolio Home](../README.md) | 🔗 [Official Nmap Guide](https://nmap.org/book/)
