# Phase 2: SIEM Dashboard Analysis

This phase focuses on integrating, visualizing, and analyzing logs from both the victim and attacker environments using Splunk. The goal is to understand the attack patterns and behaviors observed during Phase 1 and compare logs from the compromised victim with those from the attacker.

---

## ✅ Step 1: Environment Overview

- **SIEM Tool:** Splunk Enterprise (v9.3.2)
- **SIEM Location:** Kali Linux VM (Attacker)
- **Log Source:** Metasploitable3 VM (Victim)
- **Splunk Forwarder Installed:** Yes (on Metasploitable3)
- **Connection Type:** Host-Only Network
- **Monitored Log File:** `/var/log/auth.log`

> 📎 **Note:** For Splunk installation steps, see [splunk_installation.md](./splunk_installation.md)

---

## ✅ Step 2: Splunk Setup & Configuration

### 🔹 Splunk Server Setup (Kali Linux)

Splunk Enterprise was installed on the Kali Linux attacker VM using the `.deb` package. After installation, it was accessed via the web browser:

```bash
http://localhost:8000
```

The Splunk web interface was used for log visualization and analysis.

---

### 🔹 Splunk Forwarder Setup (Metasploitable3)

The Splunk Universal Forwarder was installed on Metasploitable3. After installation, it was configured to forward logs to the Splunk Server using the following commands:

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server <splunk-server-ip>:9997
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
```

Autostart on boot was enabled with:

```bash
sudo /opt/splunkforwarder/bin/splunk enable boot-start
```

---

## ✅ Step 3: Verifying Log Ingestion

Once configuration was complete, the Metasploitable3 host appeared in the **Splunk > Search & Reporting > Data Summary** panel.

Splunk confirmed it was successfully receiving log data from `/var/log/auth.log`.

A screenshot of the connected host appeared like this:

![Splunk Host Connected](./splunk_screenshots/splunk_data_summary.png)

---

## ✅ Step 4: Basic Log Queries

Initial log searches were performed in Splunk's **Search & Reporting** app using basic SPL (Search Processing Language) queries.

For example, this query was used to retrieve all logs from the Metasploitable3 VM:

```bash
index=* host="metasploitable3-ub1404"
```

This allowed us to filter and analyze logs related to the attack activity from Phase 1.

---

## ✅ Step 5: Log Analysis Overview

The Splunk dashboard allowed correlation of FTP attack activity, shell commands issued after reverse shell access, and any suspicious patterns originating from or targeting the victim machine.

Screenshots and detailed queries will be attached to demonstrate these log insights.

---

