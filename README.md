# Splunk SIEM Projects

This repository contains a collection of practical projects focused on using **Splunk as a Security Information and Event Management (SIEM)** tool for log monitoring, threat detection, and security analytics. Each project simulates real-world use cases relevant to SOC operations, log management, and incident response.

---

## Purpose

- Demonstrate the use of Splunk for analyzing system and network logs.
- Showcase integration with forwarders and custom data sources.
- Apply Splunk SPL for identifying anomalies and suspicious activities.
- Provide reproducible steps for building a hands-on SIEM lab environment.

---

## Project List

### 1. **Forwarding Windows Event Logs to Splunk Using Universal Forwarder**
- **Description**: Demonstrates how to configure the Splunk Universal Forwarder on a Windows machine to forward logs to Splunk Enterprise on an Ubuntu VirtualBox.
- **Key Features**:
  - Full setup of Universal Forwarder.
  - Monitoring `System`, `Security`, and `Application` event logs.
  - Verification and analysis of forwarded logs using SPL.
- **Highlights**:
  - Queries for login failures, account lockouts, and privilege escalations.
  - Use of real-time dashboards and alerts.
- [View Project](https://github.com/00112244/splunk_works/blob/main/Forwarding-Windows-Event-Logs-to-Splunk-Using-Universal-Forwarder.md)

---
### 2. **Analyzing DNS Log Files in Splunk**
- **Description**: Demonstrates how to upload, parse, and analyze DNS log files in Splunk SIEM to detect anomalies and investigate suspicious domains.  
- **Key Features**:  
  - Uploading sample DNS logs into Splunk.  
  - Extraction of key fields such as source IP, destination IP, query type, and response code.  
  - Use of SPL queries to identify spikes, anomalies, and suspicious domains.  
- **Highlights**:  
  - Queries for unusual DNS activity and malicious domain lookups.  
  - Identification of top queried domains and source IPs.  
  - Integration with threat intelligence feeds (VirusTotal, OTX).  
- [View Project](https://github.com/00112244/splunk_works/blob/main/Forwarding-Windows-Event-Logs-to-Splunk-Using-Universal-Forwarder.md)
---
### 3. **Analyzing SMTP & HTTP Log Files in Splunk**
- **Description:** Demonstrates how to upload, parse, and analyze **SMTP (email)** and **HTTP (web server)** log files in Splunk SIEM to monitor communication patterns, detect anomalies, and identify potential threats.
- **Key Features:**
  - Uploading sample SMTP and HTTP logs into Splunk.
  - Extraction of key fields such as sender/recipient addresses, subjects, request methods, URLs, response codes, and user agents.
  - Use of SPL queries to detect brute force attempts, phishing campaigns, SQL injections, and abnormal traffic.
- **Highlights:**
  - Queries for detecting failed SMTP logins, suspicious attachments, and unusual email activity.
  - Identification of top senders/recipients in SMTP and top endpoints or error codes in HTTP.
  - Dashboards and alerts to visualize trends and respond to anomalies in real time.
- [View Project](https://github.com/00112244/splunk_works/blob/main/Analyzing-SMTP-and-HTTP-Log-Files-Using-Splunk-SIEM.md)
---
### 4. Analyzing FTP & SSH Log Files in Splunk
- **Description:** Demonstrates how to upload, parse, and analyze FTP (file transfers) and SSH (remote access) log files in Splunk SIEM to monitor user activity, detect anomalies, and identify potential security threats.
- **Key Features:**
  - Uploading sample FTP and SSH logs into Splunk.
  - Extraction of key fields such as timestamps, usernames, source IPs, commands, file paths, and login outcomes.
  - Use of SPL queries to detect brute-force attempts, unauthorized file transfers, and abnormal login activity.
- **Highlights:**
  - Queries for analyzing file transfer frequency, top users/IPs, and unusual FTP activity.
  - Detection of failed SSH login attempts, suspicious remote access, and session anomalies.
  - Dashboards and alerts to visualize login trends, top file transfers, and real-time suspicious activity.
- [View Project](https://github.com/00112244/splunk_works/blob/main/Analyzing-FTP-and-SSH-Log-Files-Using-Splunk-SIEM.md)
---


