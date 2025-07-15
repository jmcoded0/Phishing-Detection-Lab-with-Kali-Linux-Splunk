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
- Sent Apache logs to Splunk using HEC
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

_Add these if available:_
- SET interface
- Cloned login page
- Splunk alert

---

## 🔗 Related Projects

- [AWS S3 Breach Detection Lab](https://github.com/jmcoded0/AWS-S3-Data-Breach-Simulation-Incident-Response-Lab)
- [Web App Pentesting & Vulnerability Lab](https://github.com/jmcoded0/Web-Application-Pentesting-Vulnerability-Management-Lab)

---

## ✅ Summary

This lab helped me understand how phishing works in practice and how defenders can build detection logic using Splunk and logs. It also improved my skills in network forensics and simulating real-world attacks in a safe lab environment.
