# File Transfer Techniques – Windows & Linux

## 📌 Overview
This module explores various methods for transferring files **to and from target Windows and Linux systems**. It emphasizes the use of **living off the land (LotL)** techniques, leveraging built-in operating system utilities to move files without relying on third-party tools.  

Since operating systems and security monitoring capabilities differ across environments, these techniques prepare penetration testers, red teamers, and security researchers for real-world scenarios where stealthy file transfer is required.

---

## 🔑 Key Topics
- **Living off the Land Techniques**  
  Using native system utilities for stealthy file transfers.  
- **Windows File Transfer Methods**  
  - PowerShell-based transfers  
  - `certutil.exe` for downloading files  
  - SMB & Netcat transfers  
- **Linux File Transfer Methods**  
  - `scp`, `wget`, and `curl`  
  - Bash one-liners  
  - Netcat & Python HTTP servers  
- **Exfiltration Techniques**  
  Retrieving files from compromised systems for offline analysis.  

---

## 🎯 Learning Outcomes
By completing this module, you will be able to:
1. Transfer files across Windows and Linux systems using built-in tools.  
2. Bypass restrictions by creatively leveraging native commands.  
3. Adapt file transfer techniques to different security monitoring environments.  
4. Exfiltrate sensitive files securely for analysis on your attack box.  

---

## ⚠️ Disclaimer
This repository is created **for educational and ethical purposes only**.  
The techniques demonstrated here are intended for use in controlled lab environments.  
Do **not** attempt these on systems you do not own or have explicit permission to test.  

---

## 🛠️ Requirements
- Basic knowledge of Linux & Windows command-line utilities  
- A penetration testing lab setup (e.g., Kali Linux, Windows VM)  

---

## 🚀 Getting Started
Clone the repository and explore the examples:  
```bash
git clone https://github.com/your-username/file-transfer-techniques.git
cd file-transfer-techniques
