# SOC Lab: Active Attack Simulation & Forensic Investigation

## 🛡️ Project Overview
This project demonstrates an end-to-end simulation of a cyberattack, focusing on **Initial Access**, **Execution**, and **Command & Control (C2)**. I simulated a malware-driven reverse shell attack from **Kali Linux** against a **Windows 10 target** and performed a forensic deep-dive to identify malicious artifacts.



## 🛠️ Lab Environment
* **Attacker:** Kali Linux (Metasploit Framework) (VMware)
* **Victim:** Windows 10 x64 (VMware)
* **Tools Used:** `msfvenom`, Python HTTP Server, Wireshark, Splunk, Wazuh.

---

## ⚔️ Attack Phase: Exploitation & C2
The attack scenario involved delivering a malicious payload to the victim machine to establish a remote connection.

1. **Payload Generation:** Created a Windows Reverse TCP executable (`backup_agent.exe`) using `msfvenom` configured for port 4444.
2. **Delivery:** Hosted the payload via a Python-based HTTP server.
3. **Execution:** Simulated user execution on the Windows VM, resulting in a successful **Meterpreter session**.
4. **Post-Exploitation:** Conducted file system modifications and simulated data exfiltration.

---

## 🛡️ Forensic Investigation (The SOC Perspective)
Using a variety of forensic tools, I identified the following "footprints" left by the attacker.

### 1. Network Telemetry (Wireshark)
* **Observation:** Identified a persistent TCP stream between the victim and the attacker IP on port **4444**. 
* **Analyst Note:** The ~2MB data transfer observed in the stream suggests exfiltration or secondary tool downloads.



### 2. Windows Event Log Analysis
* **Event ID 5156:** Confirmed an outbound connection was permitted from the victim to the attacker's IP on the C2 port.
* **Event ID 1001 (AppCrash):** Captured a crash report for `backup_agent.exe`, which often occurs during payload execution or migration in a lab environment.

### 3. Registry Artifacts
* **UserAssist Key:** Analyzed the registry to confirm the GUI-based execution of the malicious binary via Windows Explorer.

---

## 📊 MITRE ATT&CK Mapping
| Tactic | Technique | ID | Artifact Found |
| :--- | :--- | :--- | :--- |
| **Execution** | User Execution | T1204 | UserAssist Registry Key |
| **Command & Control** | Application Layer Protocol | T1071 | Port 4444 Traffic |
| **Exfiltration** | Exfiltration Over C2 Channel | T1041 | Wireshark TCP Stream |

---

## 📂 Repository Contents
* [Sanitized_Incident_Report](./Active_Attack_Simulation_&_Forensic_Investigation_Proof.pdf): Full technical evidence report including screenshots.
* `README.md`: Project summary and investigation notes.

---

## 🚀 Key Skills Demonstrated
* **Malware Analysis:** Understanding how payloads interact with the Windows OS.
* **Network Forensics:** Using Wireshark to identify C2 communication patterns.
* **SIEM Correlation:** Mapping Process IDs to outbound network traffic in Splunk/Wazuh.
* **Incident Reporting:** Creating a professional SOC Incident Report for stakeholders.

---
*Disclaimer: This lab was conducted in a safe, isolated virtual environment for educational and research purposes only.*
