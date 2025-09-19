# Analyzing SMTP and HTTP Log Files Using Splunk SIEM

## Introduction

Log analysis is a critical part of security monitoring.

* **SMTP (Simple Mail Transfer Protocol) logs** provide insights into email communication, such as sender/recipient addresses, timestamps, message delivery status, and authentication attempts. These logs are essential for detecting **phishing, brute force, and suspicious email behavior**.
* **HTTP (Hypertext Transfer Protocol) logs** record details of web traffic, including request methods, URLs, response codes, and user agents. These logs help identify **SQL injection attempts, directory traversal, brute force attacks, and anomalies in web traffic**.

By analyzing both **SMTP and HTTP logs** with **Splunk SIEM**, security professionals can effectively monitor network activity, detect anomalies, and identify potential threats in real time.

---

##  Project Overview

This project demonstrates how to:

* Upload and index **SMTP and HTTP log files** in Splunk.
* Use **Splunk SPL queries** to analyze patterns and detect anomalies.
* Build **dashboards** for email and web activity monitoring.
* Configure **alerts** for suspicious activity such as brute force attempts, SQL injections, and phishing campaigns.

---

##  Prerequisites

Before starting, ensure that:

* Splunk Enterprise (or Splunk Free) is installed and configured.
* Sample SMTP and HTTP log files are available in plain text format.
* Logs contain relevant events (e.g., timestamps, sender/recipient, request methods, response codes, user agents).
* Appropriate indexes and source types are configured in Splunk.

---

##  Steps to Upload Log Files in Splunk

### 1. Prepare Sample Log Files

* **SMTP logs**: Containing sender/recipient addresses, subjects, timestamps, status codes. [SMTP Log File](https://www.secrepo.com/maccdc2012/smtp.log.gz)
* **HTTP logs**: Containing timestamps, request methods, URLs, response codes, and user agents. [HTTP Log File](https://www.secrepo.com/maccdc2012/http.log.gz)
* Save the files in a directory accessible by your Splunk instance.

### 2. Upload Logs to Splunk

1. Log in to the **Splunk Web Interface**.
2. Navigate to **Settings > Add Data**.
3. Select **Upload** as the data input method.

### 3. Choose Log File

* Select the appropriate log file (`smtp_logs_sample.log` or `http_logs_sample.log`).

### 4. Set Source Type

* For **SMTP logs**, choose `mail` (or create a custom source type).
* For **HTTP logs**, choose `access_combined` (or a custom source type).

### 5. Configure Settings

* Define **index**, **host**, and **sourcetype** to match the log type.

### 6. Review and Upload

* Review all settings.
* Click **Submit** to upload the log file.

### 7. Verify Upload

* Open the **Splunk Search bar**.
* Run a query such as:

  ```spl
  index=<your_index> sourcetype=mail
  ```

  or

  ```spl
  index=<your_index> sourcetype=access_combined
  ```
* Confirm that the uploaded events are searchable.

---

## Example Analysis Use Cases

### SMTP Log Analysis

* Detect multiple **failed login attempts** (brute force attacks).
* Identify **phishing attempts** based on suspicious subjects or domains.
* Monitor **email traffic patterns** (top senders, recipients, domains).

### HTTP Log Analysis

* Detect **SQL injection attempts** (`UNION SELECT`, `' OR 1=1 --`).
* Identify **suspicious user agents** (bots, scanners).
* Track **failed login attempts** on web applications.
* Detect **unusual request patterns** (directory traversal, fuzzing).

---
# Steps to Analyze SMTP and HTTP Log Files in Splunk SIEM

This guide explains how to analyze **SMTP** and **HTTP** log files in **Splunk SIEM**. By following these steps, you can extract useful fields, identify traffic patterns, detect anomalies, and monitor user behavior.

---

##  Analyzing SMTP Log Files

### 1. Search for SMTP Events

Run a basic query to retrieve SMTP events:

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
```

### 2. Extract Relevant Fields

SMTP logs typically contain **timestamps, sender/recipient addresses, subjects, attachment details, and status codes**.
Use regex or Splunk’s field extractor to extract fields:

```spl
| rex field=_raw "<regex_pattern>"
```

### 3. Analyze Email Traffic Patterns

* **Top senders:**

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
| top limit=10 sender_address
```

* **Top recipients:**

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
| top limit=10 recipient_address
```

### 4. Detect Anomalies

* Unusual email volumes over time:

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
| timechart span=1h count
```

* Suspicious attachments:

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
| search attachment_type="exe" OR attachment_size > 1000000
```

### 5. Monitor User Behavior

* Count email activity per user:

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
| stats count by user
```

* Failed login attempts:

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
| search action="login" status="failed"
| stats count by user
```

* Daily user activity trends:

```spl
index=<your_smtp_index> sourcetype=<your_smtp_sourcetype>
| timechart span=1d count by user
```

---

##  Analyzing HTTP Log Files

### 1. Search for HTTP Events

Retrieve HTTP events with:

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
```

### 2. Extract Relevant Fields

HTTP logs typically contain **timestamps, methods, URIs, response codes, user agents, and IPs**.
Example field extraction:

```spl
| rex field=_raw "<regex_pattern>"
```

### 3. Analyze Web Traffic Patterns

* **Request methods distribution (GET, POST, etc.):**

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats count by method
```

* **Top accessed URLs:**

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| top limit=10 uri
```

* **Response code analysis:**

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats count by status
```

### 4. Detect Anomalies

* Traffic volume over time:

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| timechart span=1h count
```

* High error responses (4xx/5xx):

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats count by status
| where status >= 400
```

* Suspicious IP activity:

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| search src_ip="suspicious_ip"
```

### 5. Monitor User Behavior

* Failed login attempts:

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| search action="login" status="failed"
| stats count by user
```

* Session analysis:

```spl
index=<your_http_index> sourcetype=<your_http_sourcetype>
| stats range(_time) as session_duration by session_id
| stats avg(session_duration) as avg_session_duration by user
```

---



## Visualization & Alerts

* **Dashboards**: Build separate panels for SMTP and HTTP activity (e.g., login failures, suspicious requests, traffic trends).
* **Alerts**: Configure alerts to trigger when thresholds are exceeded (e.g., >10 failed SMTP logins in 5 minutes, multiple SQL injection attempts).

---

## Conclusion

Analyzing **SMTP and FTP** log files using Splunk SIEM provides organizations with deeper visibility into critical communication and file transfer activities across their networks. By monitoring email traffic and file transfer events, detecting anomalies, and correlating data with other logs, security teams can proactively identify and respond to threats such as phishing, brute force attempts, or unauthorized data transfers. Leveraging Splunk’s powerful search, visualization, and alerting capabilities helps ensure the integrity, confidentiality, and availability of both email communications and file exchange processes, ultimately strengthening the organization’s overall security posture.

---


