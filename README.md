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
- [View Project](./windows-event-forwarding/README.md)

---

