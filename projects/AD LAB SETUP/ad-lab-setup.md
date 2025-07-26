# 🏠 Active Directory Penetration Testing Lab  
**Duration:** Jan 2025 – Apr 2025  
**Type:** Home Lab  
**Focus:** AD setup, management, exploitation, and mitigation

---

## 🎯 Objective

To simulate a real-world enterprise Windows environment using Active Directory and practice:
- AD administration and configuration
- Offensive security techniques
- Internal network exploitation
- Privilege escalation in Windows domains

---

## 🧰 Lab Environment

| Component              | Details                          |
|------------------------|----------------------------------|
| **Virtualization Tool** | VMware Workstation 17 Pro        |
| **Server OS**          | Windows Server 2022              |
| **Client OS**          | Windows 10 Enterprise            |
| **Attacker OS**        | Kali Linux (latest version)      |
| **Network Type**       | Host-only Internal Network       |
| **Domain Name**        | `lab.local`                      |

---

## ⚙️ Configuration Steps

### Windows Server 2022 (Domain Controller)
- Installed AD DS (Active Directory Domain Services)
- Promoted server to Domain Controller (`lab.local`)
- Configured DNS and DHCP roles
- Created multiple users, groups, and Organizational Units (OUs)
- Created GPOs for:
  - Password policies
  - Login messages
  - Restricting access to Control Panel and CMD
- Set up shared folders and NTFS permissions

### Windows 10 Client(s)
- Joined the domain `lab.local`
- Tested user logins and GPO inheritance
- Verified DNS and LDAP communication with DC

---

## 🧨 Offensive Testing & Exploitation

### 🔍 Enumeration
- `nmap`, `enum4linux`, `rpcclient`, `ldapsearch`, `bloodhound`

### 🧪 Attacks Performed

| Attack Type | Tools Used | Description |
|------------|------------|-------------|
| **LLMNR/NBT-NS Poisoning** | Responder | Captured NTLMv2 hashes from misconfigured name resolution |
| **SMB Relay** | Impacket’s `ntlmrelayx.py` | Relayed authentication to gain access |
| **Kerberoasting** | Impacket + Hashcat | Requested SPNs, cracked service account hashes |
| **Token Impersonation** | PrintSpoofer, Rogue Potato | Escalated to SYSTEM privileges |
| **Credential Dumping** | Mimikatz | Extracted NTLM hashes and plaintext passwords |
| **LDAP Enumeration** | `ldapdomaindump` | Extracted user/group structure and privileges |
| **BloodHound Mapping** | SharpHound + BloodHound | Visualized attack paths and privilege escalation chains |
| **LNK File Exploitation** | Custom payload | Executed malicious shortcut in user’s startup folder |

---

## 🧠 Key Learning Outcomes

- Deep understanding of how Active Directory functions behind the scenes
- Simulated attacker mindset within an internal Windows environment
- Practiced privilege escalation using common red team techniques
- Learned defensive configurations to mitigate AD-specific attacks

---

## 🛡️ Mitigations Implemented

- Disabled LLMNR and NBT-NS via GPO
- Enforced complex password and lockout policies
- Audited privileged accounts and removed excessive rights
- Enabled PowerShell logging and command line auditing
- Deployed event log forwarding for centralized visibility

---

## 📸 Screenshots

> _Screenshots available in the `../screenshots/` folder_  
> Example: BloodHound visualization, Mimikatz hash dump, Responder logs

---

📁 [Back to Projects](../projects/README.md) | [Back to Portfolio Home](../README.md)
