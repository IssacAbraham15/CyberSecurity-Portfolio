# 🏠 Active Directory Home Lab (Jan 2025 – Apr 2025)

## 🎯 Objective

Set up a fully functional AD domain in a virtualized lab environment to practice:
- Windows Server administration
- Active Directory management
- Vulnerability identification and exploitation
- Blue-team and hardening concepts

---

## 🛠️ Lab Environment

| Component         | Details                              |
|------------------|--------------------------------------|
| Virtualization    | VMware Workstation 17 Pro            |
| Server OS         | Windows Server 2022                  |
| Clients           | Windows 10 Enterprise                |
| Attacker Machine  | Kali Linux                           |
| Admin Tools       | ADUC, GPMC, DNS, File Sharing        |
| Security Tools    | Nmap, Nessus, Nikto, Metasploit      |

---

## ⚙️ Configuration Tasks Completed

- Installed and configured **Windows Server 2022** on a VM
- Set up and managed **Active Directory Domain Services (AD DS)**
- Created and managed **users, groups, and organizational units (OUs)**
- Configured **DNS**, static IP addresses, and domain join for Windows 10 clients
- Implemented **Group Policy Objects (GPOs)** for password policy, user restrictions, and login messages
- Enabled **network file sharing** and controlled permissions

---

## 🔐 Offensive Security Practice

Simulated internal attacker activity using Kali Linux:

### 🔍 Recon & Enumeration:
- `nmap`, `enum4linux`, `smbclient`, `ldapsearch`

### 🧨 Exploitation Techniques:
- **LLMNR/NBT-NS Poisoning** using Responder
- **SMB Relay Attacks**
- **Kerberoasting** with `GetUserSPNs.py` + Hashcat
- **Token Impersonation** and `PrintSpoofer`
- **Pass-the-Hash / Pass-the-Ticket** using Mimikatz
- **BloodHound + SharpHound** for AD path analysis
- **LNK file attacks**, **NTLM hash capture**, **MSSQL misconfig**

---

## 🧰 Tools Used

- 🖥️ Virtualization: VMware Workstation 17 Pro  
- ⚙️ AD Management: ADUC, GPMC, DNS Manager  
- 🔎 Enumeration: enum4linux, ldapsearch, rpcclient  
- 🔓 Exploitation: Responder, Impacket, CrackMapExec, Mimikatz, BloodHound

---

## 🛡️ Takeaways

- Gained deep understanding of **Active Directory architecture**
- Practiced **privilege escalation and lateral movement**
- Identified mitigation strategies such as disabling LLMNR, enforcing strong Kerberos policies, and audit logging
- Strengthened both **red-team and blue-team** perspectives

---

📸 _You can find screenshots in the [`../screenshots`](../screenshots) folder._

📁 _[Return to all Projects](./README.md)_
