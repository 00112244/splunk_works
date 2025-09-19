# Analyzing Tunnel and DHCP Log Files Using Splunk SIEM

## 1. Introduction

Log analysis is critical for monitoring and securing network infrastructure. Two important sources of network visibility are:

* **Tunnel logs (from Zeek IDS):** Capture tunneling protocol activities such as GRE, IPv4, IPv6, and others, which may be used for legitimate traffic or to bypass security controls.
* **DHCP logs:** Contain information about IP address assignments, lease durations, client identifiers, and server responses, essential for tracking network devices and troubleshooting connectivity.

Analyzing both tunnel and DHCP logs in **Splunk SIEM** enables security teams and administrators to monitor network behavior, detect anomalies, and identify potential threats.

---

## 2. Project Overview

This project demonstrates how to:

* Upload and analyze **tunnel log traffic from Zeek IDS**.
* Upload and analyze **DHCP log files**.
* Extract key fields to gain insights into tunneling activities and IP address usage.
* Build queries, dashboards, and alerts to detect anomalies and improve visibility.

---

## 3. Prerequisites

Before starting, ensure:

* A running **Splunk instance** (Enterprise/Free/Trial).
* **Tunnel logs from Zeek IDS** and **DHCP logs** available in text or supported formats.
* Proper index and sourcetype configurations for accurate parsing.

---

## 4. Steps to Upload Tunnel Log Files (Zeek IDS)

1. **Prepare Tunnel Log Files**

   * Obtain sample Zeek IDS tunnel logs.
   * Ensure logs include:

     * Timestamps
     * Source and destination IPs
     * Tunneling protocols (e.g., GRE, IPv4, IPv6)

2. **Upload Logs to Splunk**

   * Navigate to **Settings > Add Data** in Splunk Web.
   * Select **Upload** as the data input method.
   * Choose the prepared tunnel log file.

3. **Set Source Type**

   * Select **bro** (Zeek) or configure a **custom sourcetype** for tunnel logs.
   * Assign index, host, and sourcetype appropriately.

4. **Review & Submit**

   * Review configurations.
   * Click **Submit** to upload.

5. **Verify Upload**

   * Run a query such as:

     ```spl
     index=tunnel_logs sourcetype=bro
     | stats count by src_ip, dest_ip, tunnel_type
     ```

---

## 5. Steps to Upload DHCP Log Files

1. **Prepare DHCP Log Files**

   * Obtain sample DHCP logs.
   * Ensure logs include:

     * Timestamps
     * IP address assignments
     * Lease durations
     * Client identifiers
     * Server responses

2. **Upload Logs to Splunk**

   * Navigate to **Settings > Add Data**.
   * Select **Upload** as the data input method.
   * Choose the prepared DHCP log file.

3. **Set Source Type**

   * Select **dhcpd** or define a **custom sourcetype**.
   * Assign index, host, and sourcetype appropriately.

4. **Review & Submit**

   * Review configurations.
   * Click **Submit** to upload.

5. **Verify Upload**

   * Run a query such as:

     ```spl
     index=dhcp_logs sourcetype=dhcpd
     | stats count by client_ip, hostname, lease_time
     ```

---

## 6. Use Cases

* **Tunnel Logs (Zeek IDS):**

  * Detect unauthorized tunneling activity.
  * Identify unusual source/destination IP pairs.
  * Monitor use of specific tunneling protocols.

* **DHCP Logs:**

  * Track IP address assignments per user/device.
  * Detect frequent lease renewals or anomalies.
  * Troubleshoot client connectivity issues.

---

## Steps to Analyze Tunnel & DHCP Log Files in Splunk SIEM

### 1. Search for Events

* Open the Splunk interface and navigate to the search bar.
* Run queries to retrieve Tunnel or DHCP events:

  * **Tunnel Logs (Zeek IDS):**

    ```spl
    index=<your_tunnel_index> sourcetype=<your_tunnel_sourcetype>
    ```
  * **DHCP Logs:**

    ```spl
    index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
    ```

---

### 2. Extract Relevant Fields

* For **Tunnel Logs**, extract: timestamps, source/destination IP addresses, and tunneling protocols (GRE, IPv4, IPv6, etc.).
* For **DHCP Logs**, extract: timestamps, IP addresses, lease durations, and client identifiers.
* Example field extraction command:

  ```spl
  | rex field=_raw "<regex_pattern>"
  ```

---

### 3. Analyze Patterns

* **Tunnel Logs:**

  * Distribution of tunneling protocols:

    ```spl
    index=<your_tunnel_index> sourcetype=<your_tunnel_sourcetype>
    | stats count by tunnel_protocol
    ```
  * Monitor GRE activity over time:

    ```spl
    index=<your_tunnel_index> sourcetype=<your_tunnel_sourcetype> tunnel_protocol=GRE
    | timechart span=1h count
    ```

* **DHCP Logs:**

  * Distribution of IP address assignments:

    ```spl
    index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
    | stats count by leased_ip
    ```
  * Identify top leased IPs:

    ```spl
    index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
    | top limit=10 leased_ip
    ```

---

### 4. Detect Anomalies

* **Tunnel Logs:**

  * Spot unusual tunneling activity with time-based patterns:

    ```spl
    index=<your_tunnel_index> sourcetype=<your_tunnel_sourcetype>
    | timechart count by tunnel_protocol
    ```

* **DHCP Logs:**

  * Detect unauthorized or unknown client requests:

    ```spl
    index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
    | search NOT client_identifier="authorized_identifier"
    ```
  * Look for abnormal patterns in address assignments:

    ```spl
    index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
    | timechart span=1h count
    ```

---

### 5. Monitor Long-Term Trends

* **Tunnel Logs:** Monitor tunneling protocol activity across longer durations to detect trends and persistent anomalies.
* **DHCP Logs:**

  * Track IP usage and lease renewals:

    ```spl
    index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
    | stats count by leased_ip, lease_renewal
    | where count > 1 AND lease_renewal="true"
    ```
  * Daily monitoring of DHCP traffic patterns:

    ```spl
    index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
    | timechart span=1d count by leased_ip
    ```

---

## Conclusion

Integrating Zeek IDS tunnel logs and DHCP log files into Splunk SIEM provides security teams with critical visibility into both tunneling activities and IP address management within a network. By monitoring these events, detecting anomalies, and correlating with other log sources, organizations can proactively identify potential threats, troubleshoot issues, and strengthen their overall cybersecurity posture.

Feel free to customize these steps according to your specific use case and requirements.

**Happy analyzing!**

