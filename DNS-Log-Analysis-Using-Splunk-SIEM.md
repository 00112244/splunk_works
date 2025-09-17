# DNS Log Analysis Using Splunk SIEM



## Introduction
DNS (Domain Name System) logs are a critical source of information for understanding network activity, troubleshooting, and detecting security threats.  
This project demonstrates how to use **Splunk SIEM** to analyze DNS logs for unusual activity, malicious domains, and other security events.

---

## Objectives
- Learn to upload and index DNS logs in Splunk.  
- Extract key fields such as source IP, destination IP, query type, and response codes.  
- Identify anomalous DNS activity.  
- Investigate suspicious domains using threat intelligence.  
- Gain hands-on experience with Splunk SIEM for DNS log monitoring.  

---

## Prerequisites
- A **Splunk instance** installed and running.  
- DNS log files available in a readable format (e.g., `.log` or `.txt`).  
- Basic knowledge of Splunk **Search Processing Language (SPL)**.  

---

## Uploading Sample DNS Log Files to Splunk

### 1. Prepare Sample DNS Log Files
- Obtain sample [DNS log file](https://www.secrepo.com/maccdc2012/dns.log.gz) in a suitable format (e.g., text files).
- Ensure the log files contain relevant DNS events, including source IP, destination IP, domain name, query type, response code, etc.
- Save the sample log files in a directory accessible by the Splunk instance.

### 2. Upload Log Files
1. Log in to the **Splunk Web Interface**.  
2. Navigate to **Settings > Add Data**.  
3. Select **Upload** as the data input method.  

### 3. Choose File
- Click **Select File** and browse to your DNS log file.

### 4. Set Source Type
- Specify the **source type** for your log file:  
  - Default: `dns`  
  - Custom: Create a new source type if needed.

### 5. Review Settings
- Configure:  
  - **Index**: e.g., `dns_index`  
  - **Host**: optional  
  - **Sourcetype**: confirm matches Step 4  

### 6. Upload
- Review settings and click **Submit** to upload.

### 7. Verify Upload
- Run a search query to confirm events are ingested:  
```spl
index=<your_dns_index> sourcetype=<your_dns_sourcetype>
```

---

## Analyzing DNS Log Files in Splunk

### 1. Search for DNS Events

```spl
index=* sourcetype=dns_sample
```

### 2. Extract Relevant Fields

* Extract key fields using `rex` or `regex`:

```spl
index=* sourcetype=dns_sample | regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"
```

### 3. Identify Anomalies

* Look for unusual patterns or spikes in queries:

```spl
index=* sourcetype=dns_sample | stats count by fqdn
```

### 4. Find Top DNS Sources

```spl
index=* sourcetype=dns_sample | top fqdn, src_ip
```

### 5. Investigate Suspicious Domains

* Search for known malicious domains:

```spl
index=* sourcetype=dns_sample fqdn="maliciousdomain.com"
```

* Integrate external reputation feeds (e.g., VirusTotal, OTX) for enhanced detection.

## Splunk DNS Logs: Using `top` with Fields

1. **Check if the field exists**

   * Before using a command like `top fqdn`, ensure the field exists in your logs:

   ```spl
   index=<your_index> sourcetype=<your_sourcetype> | table *
   ```

   * Look for a field representing domain queries (e.g., `query`, `qname`, `fqdn`).

2. **If the field doesn’t exist, extract it using `rex`**

   * Use regex to pull the domain from `_raw`:

   ```spl
   index=<your_index> sourcetype=<your_sourcetype>
   | rex field=_raw "(?<fqdn>[a-zA-Z0-9._-]+\.(com|net|org|io|in))"
   ```

   * This creates a new field called `fqdn` from the raw log text.

3. **Use the field with `top`**

   * Once the field exists, run:

   ```spl
   index=<your_index> sourcetype=<your_sourcetype> | top fqdn
   ```

   * Splunk will count the most frequent domain queries.

4. **Tip:**

   * Always verify your logs in Splunk first (`index=* | head 20`) to ensure data is being ingested.
   * If multiple domain types exist, adjust the regex in `rex` to capture all required TLDs.



---

## Example Queries

| Purpose                               | SPL Query                                              |                         |
| ------------------------------------- | ------------------------------------------------------ | ----------------------- |
| Count all DNS queries by domain       | \`index=\* sourcetype=dns\_sample                      | stats count by fqdn\`   |
| Top 10 domains queried                | \`index=\* sourcetype=dns\_sample                      | top fqdn limit=10\`     |
| Extract queries on port 53            | \`index=\* sourcetype=dns\_sample                      | regex \_raw="port 53"\` |
| Identify queries from a suspicious IP | `index=* sourcetype=dns_sample src_ip="192.168.1.100"` |                         |

---

## Identifying Anomalies and Threats

* **Unusual query volume:** Sudden spikes in requests to a specific domain.
* **Suspicious domains:** Queries to known malicious or newly registered domains.
* **Internal reconnaissance:** Abnormal patterns from internal IPs.
* **Non-standard query types:** Unexpected query types like `TXT` or `ANY`.

---

## Conclusion

Analyzing DNS logs using **Splunk SIEM** enables:

* Early detection of security incidents.
* Identification of compromised hosts or malware.
* Monitoring anomalous DNS traffic patterns.
* Enhanced visibility into network activity for proactive defense.

**Next Steps:**

* Integrate threat intelligence feeds.
* Automate alerts for suspicious activity.
* Create dashboards for real-time monitoring.

---

