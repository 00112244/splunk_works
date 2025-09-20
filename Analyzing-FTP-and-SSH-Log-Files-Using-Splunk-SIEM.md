# Analyzing FTP and SSH Log Files Using Splunk SIEM

## 1. Introduction

Log analysis is a critical component of security monitoring. Among the most frequently targeted services are **FTP (File Transfer Protocol)** and **SSH (Secure Shell)**.

* **FTP logs** provide insights into file transfer activities such as uploads, downloads, authentication attempts, and anomalies that may indicate data exfiltration or unauthorized access.
* **SSH logs** provide visibility into remote access activities, including login attempts, commands executed, and session details, which can help detect brute-force attacks or unauthorized access to critical systems.

Using **Splunk SIEM**, we can ingest, parse, and analyze these log files to improve visibility, detect threats, and set alerts for suspicious behavior.

---

## 2. Project Overview

This project demonstrates how to:

* Upload and analyze **sample FTP and SSH log files** in Splunk.
* Perform searches and create dashboards to visualize authentication attempts, session activities, and anomalies.
* Configure alerts for suspicious events (e.g., repeated failed logins, unusual file transfers, brute-force attempts).

---

## 3. Prerequisites

Before starting, ensure:

* A running **Splunk instance** (Enterprise/Free/Trial).
* Access to **sample FTP and SSH log files** in text format.
* Properly configured **log sources** (or manual uploads if working with static files).

---

## 4. Steps to Upload and Analyze FTP Log Files

### Step 1: Prepare FTP Log Files

* Collect or generate **sample FTP logs** containing:

  * Timestamps
  * Source IP addresses
  * Usernames
  * Commands (e.g., `STOR`, `RETR`)
  * Filenames accessed
* Save the files in a directory accessible to the Splunk instance.

### Step 2: Upload FTP Log Files

1. Log in to Splunk Web.
2. Go to **Settings > Add Data**.
3. Select **Upload** as the input method.
4. Choose the prepared [FTP log file](https://www.secrepo.com/maccdc2012/ftp.log.gz).

### Step 3: Configure Source Type

* Set the source type to **ftp** or define a **custom sourcetype** if needed.
* Specify index, host, and sourcetype settings.

### Step 4: Review & Submit

* Review settings.
* Click **Submit** to upload.

### Step 5: Verify Upload

* Use the **Search & Reporting App** in Splunk.
* Run queries like:

```spl
index=ftp_logs sourcetype=ftp | stats count by user, src_ip, command
```

---

## 5. Steps to Upload and Analyze SSH Log Files

### Step 1: Prepare SSH Log Files

* Collect **sample SSH logs** containing:

  * Timestamps
  * Source IP addresses
  * Usernames
  * Login/logout actions
  * Failed/successful attempts
* Save in a directory accessible to Splunk.

### Step 2: Upload SSH Log Files

1. Log in to Splunk Web.
2. Go to **Settings > Add Data**.
3. Select **Upload** as the input method.
4. Choose the [SSH log file](https://www.secrepo.com/maccdc2012/ssh.log.gz).

### Step 3: Configure Source Type

* Set the source type to **syslog** (default for SSH logs) or define a **custom sourcetype**.
* Specify index, host, and sourcetype.

### Step 4: Review & Submit

* Review and confirm.
* Click **Submit** to upload.

### Step 5: Verify Upload

* Use Splunk Search queries such as:

```spl
index=ssh_logs sourcetype=syslog "sshd" | stats count by src_ip, user, action
```

---

## 6. Dashboards & Alerts

* **Dashboards** can be created for:

  * Successful vs. failed SSH logins
  * Top source IPs for FTP transfers
  * Most accessed files via FTP
  * Time-based login trends

* **Alerts** can be configured for:

  * Repeated failed SSH login attempts from the same IP
  * Unusual volume of FTP transfers in a short period
  * Login attempts from blacklisted IPs

---

# Guide: Analyzing FTP and SSH Log Files in Splunk SIEM

This guide provides step-by-step instructions for analyzing **FTP** and **SSH** log data in Splunk SIEM. The goal is to extract meaningful insights, detect anomalies, and monitor user behavior effectively.

---

## 1. Analyzing FTP Log Files

### Step 1: Search for FTP Events

Run the following query in Splunk Search to retrieve FTP log events:

```spl
index=<your_ftp_index> sourcetype=<your_ftp_sourcetype>
```

---

### Step 2: Extract Relevant Fields

Key fields in FTP logs include:

* Timestamp
* Source IP address
* Username
* FTP command (`STOR`, `RETR`, etc.)
* File path

Example extraction command:

```spl
| rex field=_raw "^(?<timestamp>\d{4}-\d{2}-\d{2}\s+\d{2}:\d{2}:\d{2}).*?(?<source_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}).*?(?<username>\w+).*?(?<command>[A-Z]+).*?(?<file_path>\/[\w\/.-]+)"
```

**Explanation:**

* `timestamp`: Captures time in `YYYY-MM-DD HH:MM:SS` format
* `source_ip`: Extracts the client IP
* `username`: Extracts the logged-in user
* `command`: Extracts the FTP command (upload/download)
* `file_path`: Extracts the file path

---

### Step 3: Analyze File Transfer Activity

Use Splunk statistics commands to analyze transfers:

* Frequency & volume of transfers
* Top users/IPs involved
* Types of files transferred

```spl
index=<your_ftp_index> sourcetype=<your_ftp_sourcetype>
| stats count by user, source_ip, command, file_path
```

---

### Step 4: Detect Anomalies

Look for unusual behavior such as:

* Sudden spikes/drops in file transfers
* Transfers to/from suspicious IPs
* Large file movements

```spl
index=<your_ftp_index> sourcetype=<your_ftp_sourcetype>
| timechart span=1h count by command
```

---

### Step 5: Monitor User Behavior

* Track failed login attempts
* Identify unusual access attempts
* Detect deviations from normal user activity

```spl
index=<your_ftp_index> sourcetype=<your_ftp_sourcetype> "login failed"
| stats count by user, source_ip
```

---

## 2. Analyzing SSH Log Files

### Step 1: Search for SSH Events

Run the following query in Splunk Search:

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype>
```

---

### Step 2: Extract Relevant Fields

Key fields in SSH logs include:

* Timestamp
* Source IP address
* Username
* Action (login, logout, failed attempt)
* Commands executed

Example extraction command:

```spl
| rex field=_raw "<regex_pattern>"
```

---

### Step 3: Analyze SSH Activity Patterns

* **Distribution of commands executed:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype>
| stats count by command
```

* **Top users and IPs:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype>
| top limit=10 user src_ip
```

* **Successful vs. failed logins:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype>
| stats count by action
```

---

### Step 4: Detect Anomalies

* **Spikes in login attempts:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype>
| timechart span=1h count
```

* **Failed login attempts:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype> action="failed"
| stats count by src_ip, user
```

* **Logins from suspicious IPs:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype> src_ip="suspicious_ip"
```

---

### Step 5: Monitor User Behavior

* **Multiple failed login attempts per user:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype> action="failed"
| stats count by user
```

* **Analyze session durations:**

```spl
index=<your_ssh_index> sourcetype=<your_ssh_sourcetype>
| stats range(_time) as session_duration by session_id
| stats avg(session_duration) as avg_session_duration by user
```

---

##  Conclusion

Analyzing **FTP** and **SSH** log files using Splunk SIEM provides organizations with deep visibility into two critical areas of network security:

* **FTP analysis** helps track file transfer activities, detect anomalies, and uncover potential data exfiltration attempts.
* **SSH analysis** enables monitoring of remote access, detection of brute-force or unauthorized login attempts, and identification of suspicious user activity.

By continuously monitoring these events, detecting anomalies, and correlating them with other log sources, organizations can strengthen their security posture, respond faster to threats, and protect critical assets from compromise.

Feel free to adapt the outlined steps and queries to your specific use case and environment.

**Happy Analyzing with Splunk SIEM!** 🚀

