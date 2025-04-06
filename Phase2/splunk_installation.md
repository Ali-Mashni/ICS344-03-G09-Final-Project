# Splunk Installation Guide

This document explains the installation process of the Splunk Server (SIEM) and Splunk Forwarder used in Phase 2 of the ICS344 project.

---

## 🖥️ Splunk Server (Kali Linux)

###  1. Download Splunk Enterprise `.deb` package:

```bash
wget -O splunk-9.3.2.deb https://download.splunk.com/products/splunk/releases/9.3.2/linux/splunk-9.3.2-d8bb32809498-linux-2.6-amd64.deb
```

###  2. Install the package:

```bash
sudo dpkg -i splunk-9.3.2.deb
sudo apt --fix-broken install
```

###  3. Start Splunk and accept the license:

```bash
sudo /opt/splunk/bin/splunk start --accept-license
```

###  4. Enable auto-start on boot (optional):

```bash
sudo /opt/splunk/bin/splunk enable boot-start
```

---

## 🖥️ Splunk Forwarder (Metasploitable3)

###  1. Download the forwarder `.deb` package:

```bash
wget -O splunkforwarder-9.3.2.deb https://download.splunk.com/products/universalforwarder/releases/9.3.2/linux/splunkforwarder-9.3.2-d8bb32809498-linux-2.6-amd64.deb
```

###  2. Install the package:

```bash
sudo dpkg -i splunkforwarder-9.3.2.deb
sudo apt --fix-broken install
```

###  3. Start the Splunk Forwarder:

```bash
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```

###  4. Connect it to the Splunk Server:

```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 192.168.168.128:9997
```

###  5. Add logs to monitor:

```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
```
📘 **Why monitor `/var/log/auth.log`?**

This file contains **authentication-related system logs**, such as SSH logins, sudo usage, user creation attempts, failed login attempts, and more. Since Phase 1 involved remote access and possible privilege escalation, this file is essential to:

- Track reverse shell activity (e.g., new session creation)
- Observe failed login attempts and brute-force attacks
- Identify system-level security events caused by the attack

###  6. Enable auto-start on boot (optional):

```bash
sudo /opt/splunkforwarder/bin/splunk enable boot-start
```

---

## 🧪 Testing the Setup

To verify connectivity between the forwarder and the server:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

Expected Output:
![Splunk Forwarder Verification](./phase2_screenshots/forward_list.png)