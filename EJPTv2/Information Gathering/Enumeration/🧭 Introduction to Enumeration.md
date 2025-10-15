## 📘 Overview
Enumeration is the phase after **host discovery** and **port scanning** in a penetration test.  
It focuses on gathering **detailed information** about network hosts and the services running on them.

---

## 🎯 Purpose
- Identify detailed service configurations and user information  
- Collect data such as:
  - Account names  
  - Shared resources  
  - Misconfigured or vulnerable services  

> Enumeration is **active** — it requires direct connections to remote systems.

---

## ⚙️ Enumeration Targets
Attackers focus on network protocols that are **misconfigured** or **left enabled** unnecessarily.

| Service | Protocol | Default Port | Description |
|----------|-----------|---------------|--------------|
| FTP | TCP | 21 | File transfer service |
| SMB | TCP | 445 / 139 | Network file sharing |
| HTTP | TCP | 80 | Web server communication |
| MySQL | TCP | 3306 | Database management system |
| SSH | TCP | 22 | Secure remote administration |
| SMTP | TCP | 25 | Email transmission |

---

## 🔁 Enumeration in Penetration Testing Methodology
### **Process Flow**
1. **Information Gathering**
   - Passive: OSINT  
   - Active: Network Mapping, Host Discovery, Port Scanning  
2. **Enumeration**
   - Service & OS Enumeration  
   - User Enumeration  
   - Share Enumeration  
3. **Exploitation**
4. **Post-Exploitation**
5. **Reporting**

---

## 🧰 Tools and Techniques

### **Nmap**
- Free, open-source network scanner  
- Identifies live hosts, open ports, and running services  
- Performs service detection and OS detection  
- Output can be exported and imported into **Metasploit Framework (MSF)** for further exploitation

### **Metasploit Auxiliary Modules**
- Used for **scanning**, **discovery**, and **fuzzing**
- Can perform:
  - TCP & UDP port scanning  
  - Service enumeration (FTP, SSH, HTTP, etc.)
- Useful during both **information gathering** and **post-exploitation** (for pivoting or scanning internal subnets)

---

## 🧩 Key Takeaways
- Enumeration bridges **scanning** and **exploitation**
- Converts raw network data into actionable intelligence
- Common tools:
  - **Nmap**
  - **Metasploit auxiliary modules**
  - **Protocol-specific scripts**
- Nmap output formats (e.g., `.xml`, `.nmap`) can be imported into other tools for vulnerability analysis

---

**Source:** INE – *Assessment Methodologies: Enumeration (Lecture 1)*  
**Instructor:** Alexis Ahmed, Senior Penetration Tester @ HackerSploit
