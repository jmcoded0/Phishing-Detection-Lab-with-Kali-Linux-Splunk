# 🎣 Phishing Detection Lab with Kali Linux & Splunk

This is a hands-on cybersecurity lab where I simulated a phishing attack using Kali Linux and detected it using Splunk. I built a fake login page with the Social-Engineer Toolkit (SET), captured credentials, and monitored the event via Apache logs in Splunk.

---

## 📚 Lab Overview

Phishing is one of the most common attack vectors used in real-world breaches. In this lab, I explored how attackers deliver fake login pages and how defenders can detect those attacks through proper logging and analysis.

---

## 🔧 Tools & Technologies

- Kali Linux (with SET toolkit)
- Apache2 (used to host the phishing site)
- Splunk (used to monitor access logs)
- VirtualBox Lab Environment

---

## 💡 What I Did

- Simulated a phishing campaign with SET
- Cloned a real login page and hosted it locally
- Captured victim credentials securely in a sandbox
- Forwarded phishing credentials to Splunk using HEC for analysis
- Created a Splunk detection query to flag suspicious POST requests

---

## 🎯 Key Skills Applied

- Phishing simulation
- Splunk detection & alerting
- MITRE ATT&CK mapping
- Log analysis with Apache2
- Network & HTTP traffic inspection

---

## 🧠 MITRE ATT&CK Techniques

| Tactic           | Technique         | ID        |
|------------------|-------------------|-----------|
| Initial Access   | Phishing          | T1566     |
| Collection       | Input Capture     | T1056.001 |
| Defense Evasion  | Valid Accounts    | T1078     |



---
## 📸 Screenshots

- Full walkthrough with screenshots 👉 [View full documentation here](https://github.com/jmcoded0/Phishing-Detection-Lab-with-Kali-Linux-Splunk/blob/main/documenting.md)

