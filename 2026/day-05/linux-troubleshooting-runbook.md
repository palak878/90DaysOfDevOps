I have organized your drill data into a clear, structured report. I kept your exact data, commands, and observations intact, only adding headers to make each section clearly visible.

---

## 🛠️ **Troubleshooting Drill: SSH Service**

### **1. Environment Basics**

* **Command:** `uname -a`
* `Linux ip-172-31-44-230 6.14.0-1018-aws #18~24.04.1-Ubuntu SMP Mon Nov 24 19:46:27 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux`


* **Command:** `lsb_release -a`
* `Distributor ID: Ubuntu | Description: Ubuntu 24.04.3 LTS | Release: 24.04 | Codename: noble`


* **Command:** `cat /etc/os-release`
* `PRETTY_NAME="Ubuntu 24.04.3 LTS" | VERSION_CODENAME=noble`



---

### **2. Filesystem Sanity**

* **Action:** `mkdir /tmp/runbook-demo` (Creates the throwaway folder).
* **Command:** `cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo`
* `total 4 -rw-r--r-- 1 ubuntu ubuntu 221 Feb 13 17:46 hosts-copy`


* **Observation:** Copy successful. This proves the system can write to the disk and permissions are functional.

---

### **3. CPU / Memory**

* **Identify PID:** `pgrep sshd` → `1031, 1032, 1142`
* **Process Stats:** `ps -o pid,pcpu,pmem,comm -p 1031`
* `PID 1031 | %CPU 0.0 | %MEM 0.8 | COMMAND sshd`


* **Observation:** %CPU is 0.0; the service is healthy and idling.
* **Global RAM Check:** `free -h`
* `Mem: 914Mi Total | 360Mi Used | 340Mi Free | 553Mi Available`


* **Observation:** System is not running out of RAM; plenty of "Available" memory for SSH to function.

---

### **4. Disk / IO**

* **Command:** `df -h`
* `/dev/root | Size 6.8G | Used 1.8G | Avail 5.0G | Use% 27% | Mounted on /`


* **Observation:** Use% is 27%. Well below the 90% "danger zone."
* **Log Size:** `sudo du -sh /var/log` → `17M /var/log`
* **Observation:** Logs are staying small and healthy.
* **IO Stats:** `vmstat 1 2`
* `id 99 (Idle) | bi 146 | bo 102`


* **Observation:** High idle percentage (99-100%) confirms the disk is not under heavy load.

---

### **5. Network**

* **Command:** `ss -tulpn`
* `tcp LISTEN 0 4096 0.0.0.0:22`
* `tcp LISTEN 0 4096 [::]:22`


* **Observation:** SSH daemon is in a `LISTEN` state on port 22 for both IPv4 and IPv6. Reachable.
* **Command:** `ping -c 3 localhost`
* `3 received, 0% packet loss, time 2034ms`


* **Observation:** Local connectivity is perfect. The OpenSSH banner is visible and responding.

---

### **6. Logs**

* **Command:** `sudo journalctl -u ssh -n 50`
* **Observation:** Confirmed "Server listening on 0.0.0.0 port 22"; service started successfully.
* **Command:** `sudo tail -n 50 /var/log/auth.log`
* **Observation:** Verified successful login events; no evidence of brute-force attacks.

---

### **7. If This Worsens (Next Steps)**

* **Step 1: Restart & Syntax Strategy**
* Before restarting, run `sudo sshd -t` to ensure the configuration is valid.
* Command: `sudo systemctl restart ssh`


* **Step 2: Expanded Log Analysis (Deep Audit)**
* Command: `sudo journalctl -u ssh -n 200`


* **Step 3: Firewall & Security Group Verification**
* Verify local rules with `sudo ufw status` and check AWS Security Group settings for Port 22.



---

**Would you like me to show you how to automate these exact commands into a "Health Check" script for your future projects?**
