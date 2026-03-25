# Analyzing DHCP Log Files Using Splunk SIEM

## Introduction

DHCP (Dynamic Host Configuration Protocol) log files contain valuable information about IP address assignments, lease durations, client requests, and server responses. Analyzing DHCP logs using Splunk SIEM enables network administrators to monitor IP address usage, detect anomalies, and troubleshoot network issues effectively.---

## Prerequisites

Before analyzing DNS logs in Splunk, ensure the following:

- Splunk instance is installed and configured.
- DHCP log data sources are configured to forward logs to Splunk.

---

# Steps to Upload Sample DNS Log Files to Splunk SIEM

## 1. Prepare Sample DNS Log Files

- Obtain a sample DHCP log file in a suitable format (e.g., plain text).
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

Click **Select File** and choose the DHCP log file you prepared.

---

## 4. Set Source Type

- In **Set Source Type**, specify the sourcetype for the FTP logs.  
- Use an existing type (e.g., `dhcpd`) or define a custom one.

---

## 5. Review Settings

Verify:

- **index**  
- **host**  
- **sourcetype**

Make sure they match your DHCP log sample.

---

## 6. Upload the Log File

1. Click **Review**.  
2. Confirm all settings.  
3. Click **Submit** to upload the log file.

---

## 7. Verify the Upload

Use the search bar to confirm DHCP events were ingested:

```spl
index=<your_dns_index> sourcetype=<your_dns_sourcetype>
```

---

# Steps to Analyze DNS Log Files in Splunk SIEM

## 1. Search for DNS Events
Open Splunk interface and navigate to the search bar. Enter the following search query to retrieve DNS events

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
```

---

## 2. Extract Relevant Fields
Use Splunk’s field extraction capabilities or regular expressions to extract these fields for better analysis.

- src_ip  
- dst_ip  
- fqdn  
- query_type  
- response_code

Use rex to locate DHCP-related keywords:

```spl
| rex field=_raw "<regex_pattern>"
```

---

## 3. Analyze Email Traffic Patterns

- Determine the distribution of IP address assignments

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
| stats count by leased_ip
```

- Identify top IP addresses leased by the DHCP server

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
| top limit=10 leased_ip
```

---

## 4. Detect Anomalies
- Look for unusual patterns in IP address assignments

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
| timechart span=1h count by _time
```

- Analyze DHCP requests from unauthorized or unknown clients

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
| search NOT client_identifier="authorized_identifier"
```

---

## 5. Monitor IP Address Usage

- Monitor IP address usage over time 

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
| timechart span=1h count by leased_ip
```

- Identify IP addresses with multiple lease renewals or changes

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
| stats count by leased_ip, lease_renewal
| where count > 1 AND lease_renewal="true"
```

- Analyze DHCP traffic patterns and deviations from normal behavior

```spl
index=<your_dhcp_index> sourcetype=<your_dhcp_sourcetype>
| timechart span=1d count by leased_ip
```
---

# Conclusion

Analyzing DHCP log files using Splunk SIEM provides valuable insights into IP address assignment within a network. By monitoring DHCP events, detecting anomalies, and correlating with other logs, organizations can enhance their network management capabilities, troubleshoot issues, and improve overall network security.
Feel free to adapt these steps to your environment and data sources.

Happy analyzing!
