# Analyzing DNS Log Files Using Splunk SIEM

## Introduction

DNS (Domain Name System) logs are crucial for understanding network activity and identifying potential security threats. Splunk SIEM (Security Information and Event Management) provides powerful capabilities for analyzing DNS logs and detecting anomalies or malicious activities.

---

## Prerequisites

Before analyzing DNS logs in Splunk, ensure the following:

- Splunk instance is installed and configured.
- DNS log data sources are configured to forward logs to Splunk.

---

# Steps to Upload Sample DNS Log Files to Splunk SIEM

## 1. Prepare Sample DNS Log Files

- Obtain a sample DNS log file in a suitable format (e.g., plain text).
- Ensure the log file contains relevant DNS events:  
  - source IP  
  - destination IP  
  - domain name  
  - query type  
  - response code  
- Save the sample log file in a directory accessible by the Splunk instance.

---

## 2. Upload Log Files to Splunk

1. Log in to the Splunk Web interface.  
2. Navigate to **Settings → Add Data**.  
3. Select **Upload** as the data input method.

---

## 3. Choose File

Click **Select File** and choose the DNS log file you prepared.

---

## 4. Set Source Type

- In **Set Source Type**, specify the sourcetype for the DNS logs.  
- Use an existing type (e.g., `dns`) or define a custom one.

---

## 5. Review Settings

Verify:

- **index**  
- **host**  
- **sourcetype**

Make sure they match your DNS log sample.

---

## 6. Upload the Log File

1. Click **Review**.  
2. Confirm all settings.  
3. Click **Submit** to upload the log file.

---

## 7. Verify the Upload

Use the search bar to confirm DNS events were ingested:

```spl
index=<your_dns_index> sourcetype=<your_dns_sourcetype>
```

---

# Steps to Analyze DNS Log Files in Splunk SIEM

## 1. Search for DNS Events
Open Splunk interface and navigate to the search bar. Enter the following search query to retrieve DNS events

```spl
index=* sourcetype=dns_sample
```

---

## 2. Extract Relevant Fields
As mentioned below, | regex _raw="(?i)\b(dns|domain|query|response|port 53)\b": This regex searches for common DNS-related keywords in the raw event data.
Common DNS fields:

- src_ip  
- dst_ip  
- fqdn  
- query_type  
- response_code

Use regex to locate DNS-related keywords:

```spl
index=* sourcetype=dns_sample 
| regex _raw="(?i)\b(dns|domain|query|response|port 53)\b"
```

---

## 3. Identify Anomalies

Look for spikes or unusual patterns:

```spl
index=* sourcetype=dns_sample 
| stats count by fqdn
```

---

## 4. Find the Top DNS Sources
Use the top command to count the occurrences of each query type
```spl
index=* sourcetype=dns_sample 
| top fqdn src_ip
```

---

## 5. Investigate Suspicious Domains

Search for known malicious or suspicious FQDNs.  

```spl
index=* sourcetype=dns_sample fqdn="maliciousdomain.com"
```

---

# Conclusion

Analyzing DNS log files using Splunk SIEM enables security teams to detect and respond to potential security incidents effectively. By understanding DNS activity, extracting key fields, and identifying anomalies, organizations can strengthen their security posture and respond quickly to threats.

Feel free to adapt these steps to your environment and data sources.

Happy analyzing!
