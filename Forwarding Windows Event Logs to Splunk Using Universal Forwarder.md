# Forwarding Windows Event Logs to Splunk Using Universal Forwarder on Ubuntu VirtualBox

## Overview

This project demonstrates how to collect and forward Windows Event Logs to Splunk for centralized monitoring, utilizing **Splunk Universal Forwarder** installed on a Windows machine and a **Splunk Enterprise instance** running on **Ubuntu in VirtualBox**. The integration enables real-time log aggregation and analysis, facilitating proactive security monitoring and system auditing.

This setup is particularly useful for simulating a **Security Information and Event Management (SIEM)** environment in a lab or training context.

---

## Objectives

* Install and configure Splunk Enterprise on Ubuntu VirtualBox.
* Install and configure the Splunk Universal Forwarder on Windows.
* Forward key Windows Event Logs (System, Security, Application) to Splunk.
* Verify data ingestion and perform search queries in Splunk for event analysis.
* Apply filtering and field extraction for effective threat detection.

---

## Prerequisites

* Ubuntu 20.04+ VirtualBox instance with Splunk Enterprise installed.
* Windows 10/11 or Server edition with administrator access.
* Local network access between the Windows machine and Ubuntu VirtualBox.
* Splunk Universal Forwarder installer for Windows.
* Basic knowledge of Splunk interface and search processing language (SPL).

---

## Environment Setup

### Step 1: Install Splunk Enterprise on Ubuntu

1. Download Splunk Enterprise from the [official website](https://www.splunk.com/en_us/download/splunk-enterprise.html).

2. Install the `.deb` package and start the Splunk service:

   ```bash
   sudo dpkg -i splunk_package_name.deb
   sudo /opt/splunk/bin/splunk start --accept-license
   ```

3. Create an admin account during setup.

4. Access Splunk Web UI at: `http://<ubuntu_ip>:8000`.

---

### Step 2: Enable Receiving Port on Splunk (Ubuntu)

1. In the Splunk Web Interface:

   * Navigate to **Settings > Data Inputs > Forwarded Data > Add new**.
   * Choose TCP and set port to **9997** (default forwarder port).

2. Confirm the listener is active by running:

   ```bash
   sudo netstat -tulnp | grep 9997
   ```

---

### Step 3: Install and Configure Splunk Universal Forwarder on Windows

1. Download the Universal Forwarder from the [Splunk Downloads Page](https://www.splunk.com/en_us/download/universal-forwarder.html).

2. During installation:

   * Set the indexer IP to the Ubuntu VM IP.
   * Use port **9997**.
   * Select event logs to monitor: System, Security, and Application.

3. Post-install, verify the forwarder is running:

   ```powershell
   Get-Service splunkforwarder
   ```

4. You can also manually configure inputs and outputs in:

   * `C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf`
   * `outputs.conf`

---

## Verification

### Step 1: Confirm Data Arrival in Splunk

* Go to **Search & Reporting** in Splunk.

* Run the following base query:

  ```spl
  index=* host=<windows_hostname>
  ```

* Use filters such as `source="WinEventLog:Security"` to narrow down events.

---

## Common Splunk Search Queries

### Retrieve All Windows Event Logs

```spl
index=* source="WinEventLog:*" host=<windows_hostname>
```

---

### Failed Login Attempts

```spl
index=* source="WinEventLog:Security" EventCode=4625
```

**EventCode 4625** indicates failed logon attempts.

---

### Successful Login Events

```spl
index=* source="WinEventLog:Security" EventCode=4624
```

---

### Account Lockouts

```spl
index=* source="WinEventLog:Security" EventCode=4740
```

---

### Privilege Escalation Events

```spl
index=* source="WinEventLog:Security" EventCode=4672
```

**EventCode 4672** indicates a logon with administrative privileges.

---

### Service Installation or Modification

```spl
index=* source="WinEventLog:Security" EventCode=7045
```

---

### Filter Specific User Activity

```spl
index=* source="WinEventLog:Security" user="Administrator"
```

---

### Top Event Sources

```spl
index=* source="WinEventLog:*" | top host
```

---

### Count of Events by Type

```spl
index=* source="WinEventLog:*" | stats count by source
```

---

### Timeline of Security Events

```spl
index=* source="WinEventLog:Security" | timechart count by EventCode
```

---

## Benefits of This Setup

* **Centralized Log Management**: Aggregate logs from multiple systems in one place.
* **Cross-Platform Integration**: Combine Windows and Linux environments.
* **Real-Time Analysis**: Detect anomalies and respond to threats promptly.
* **SIEM Simulation**: Create a lab environment for security analytics training.

---

## Possible Extensions

* Configure alerts in Splunk for high-risk EventCodes (e.g., 4625, 4740).
* Develop custom dashboards for real-time event monitoring.
* Integrate with threat intelligence feeds to detect known indicators of compromise (IOCs).
* Add Linux servers or endpoints for multi-platform log forwarding.

---

## Troubleshooting Tips

* Ensure no firewall blocks the 9997 port between Windows and Ubuntu.
* Use `splunk list forward-server` on the Windows forwarder to verify connection status.
* Ensure the Universal Forwarder has read permissions for Event Logs.
* Validate Splunk indexes and sourcetypes are properly configured.

---

## Conclusion
By forwarding Windows Event Logs to Splunk using the Universal Forwarder on an Ubuntu VirtualBox, this project highlights the power of integrating cross-platform systems for centralized log analysis. It enables more effective monitoring, faster threat detection, and streamlined incident response workflows.

This foundational setup can be extended to include dashboards, correlation rules, and alerts, making it an excellent entry point into SIEM operations.

Feel free to customize these steps according to your specific use case and requirements.
