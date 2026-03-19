# Open-Source-IDS
This document is a detailed report on different types of open source IDS 
# Open-Source Intrusion Detection Systems — Research & Lab Report

**Course:** CYB 623: Network Security and Defense  
**Institution:** Pace University — Seidenberg School of Computer Science and Information Systems  
**Authors:** Justin Castronuovo, Sahithi Dommeti, Mika Patel, Rayyan Baig Mirza, Andrew Peña  
**Date:** May 5, 2025

---

## Overview

This project explores the functionality and real-world deployment of four open-source Intrusion Detection Systems (IDS): **Tripwire**, **Snort**, **Suricata**, and **OSSEC**. Each tool was installed, configured, and tested in a controlled virtual lab environment using Ubuntu 20.04 VMs and Docker networks sourced from [SEED Labs](https://seedsecuritylabs.org/).

The report covers both theory and hands-on simulation — walking through detection methodologies, NIDS vs. HIDS distinctions, lab setup steps, attack simulations, and analysis of generated alerts and reports.

📄 **[View Full Report (PDF)](./IDS_Report_CYB623.pdf)**

---

## Tools Covered

| Tool | Type | Detection Method | Key Capability |
|------|------|-----------------|----------------|
| **Tripwire** | HIDS | Anomaly-based | File integrity monitoring via encrypted database snapshots |
| **Snort** | NIDS | Signature-based | Real-time packet inspection; rule-based alert engine |
| **Suricata** | NIDS/IPS | Signature + multi-mode | Multi-threaded deep packet inspection; JSON/SIEM integration |
| **OSSEC** | HIDS | Log-based / Anomaly | Real-time log analysis, severity-based alerting, active response |

---

## Lab Environments

All simulations were conducted on Ubuntu 20.04 (SEED Labs distribution) using VirtualBox and Docker.

- **Tripwire** — Single Docker container; tested file integrity detection by modifying monitored files and comparing Tripwire reports before and after
- **Snort** — Two-container Docker network (attacker + victim); simulated Nmap SYN scans and observed real-time alert generation on eth0
- **Suricata** — VirtualBox VM with bridged networking; tested built-in rule detection via a known malicious URL and created a custom HTTP content rule targeting specific keywords
- **OSSEC** — Bare Ubuntu VM (no Docker); simulated brute-force login attempts, unauthorized `su` escalation, and new user creation to trigger severity-graded alerts

---

## Key Findings

- **Tripwire** is the most accessible starting point for IDS work — straightforward setup focused on file integrity monitoring with encrypted policy and database files
- **Snort** detected ICMP pings and TCP SYN scans from the attacker container, classifying them as *Misc activity* and *Attempted Information Leak*
- **Suricata** successfully triggered alerts from both its default Emerging Threats ruleset and a custom `local.rules` file; its multi-threaded architecture makes it faster than Snort at scale
- **OSSEC** captured failed `sudo` attempts (Rule 5404, Level 10), failed `su root` logins, password changes, and a privilege escalation attempt via `usermod -aG sudo` — all logged with timestamps and severity levels in `/var/ossec/logs/alerts/alerts.log`

---

## Recommended Learning Path

For anyone getting started with open-source IDS tools:

1. **Tripwire** — Learn file integrity monitoring and how policies define violations
2. **Snort** — Move to network-based detection with a flexible rule language
3. **Suricata** — Apply similar Snort-compatible rule syntax with better performance and SIEM-ready JSON output
4. **OSSEC** — Add host-level log monitoring and active response capabilities

---

## References

- Du, W. SEED Labs: Hands-on Labs for Security Education. https://seedsecuritylabs.org/
- IBM. What is an Intrusion Detection System? https://www.ibm.com/think/topics/intrusion-detection-system
- Suricata Quickstart Guide. https://docs.suricata.io/en/latest/quickstart.html
- Guo, J., et al. (2022). Research on High Performance Intrusion Prevention System Based on Suricata. *Highlights in Science, Engineering and Technology*, 7, 238–245.
- Roesch, M. (1999). Snort – Lightweight Intrusion Detection for Networks. USENIX LISA.
- OSSEC Documentation. https://www.ossec.net/docs/
