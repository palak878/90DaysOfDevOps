******Troubleshooting Drill********

Environment basics

👉uname -a

Linux ip-172-31-44-230 6.14.0-1018-aws #18~24.04.1-Ubuntu SMP Mon Nov 24 19:46:27 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

Identifies your MINGW64 environment.

👉lsb_release -a

No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.3 LTS
Release:        24.04
Codename:       noble

👉cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.3 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo

Filesystem sanity
👉mkdir /tmp/runbook-demo: Creates the throwaway folder.

👉 cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
total 4
-rw-r--r-- 1 ubuntu ubuntu 221 Feb 13 17:46 hosts-copy

Copy the system's network configuration file to my project folder and immediately show me the result to prove it worked

*******CPU / Memory******

👉pgrep sshd
1031
1032
1142

👉ps -o pid,pcpu,pmem,comm -p 1031

    PID %CPU %MEM COMMAND
    1031  0.0  0.8 sshd
➡️Observation : Look at %CPU. If it's 0.0, the service is healthy and idling.

==We check if the whole server is running out of RAM, which could cause SSH to lag==

👉 free -h
               total               used        free      shared      buff/cache   available
Mem:       914Mi       360Mi        340Mi        2.7Mi        371Mi           553Mi
Swap:       0B              0B            0B

********Disk / IO***********

👉df -h
Filesystem       Size  Used Avail Use% Mounted on
/dev/root        6.8G  1.8G  5.0G  27% /
tmpfs            458M     0  458M   0% /dev/shm
tmpfs            183M  876K  182M   1% /run
tmpfs            5.0M     0  5.0M   0% /run/lock
efivarfs         128K  3.6K  120K   3% /sys/firmware/efi/efivars
/dev/nvme0n1p16  881M   89M  730M  11% /boot
/dev/nvme0n1p15  105M  6.2M   99M   6% /boot/efi
tmpfs             92M   12K   92M   1% /run/user/1000

➡️Observation: Check the Use%. If it's over 90%, you are in the "danger zone."

👉 sudo du -sh /var/log
17M     /var/log

➡️Observation: This tells you if your logs are staying small and healthy or if they are ballooning in size.

👉vmstat 1 2
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 2  0      0 348780  18144 363096    0    0   146   102   75    0  0  0 99  0  0  0
 0  0      0 348780  18144 363136    0    0     0     0  179  116  0  0 100  0  

➡️Observation: Look at the bi (blocks in) and bo (blocks out) columns. If these numbers are very high, the disk is under heavy load.


*******Network******
👉 ss -tulpn
Netid State  Recv-Q Send-Q       Local Address:Port   Peer Address:Port Process
udp   UNCONN 0      0               127.0.0.54:53          0.0.0.0:*
udp   UNCONN 0      0            127.0.0.53%lo:53          0.0.0.0:*
udp   UNCONN 0      0       172.31.44.230%ens5:68          0.0.0.0:*
udp   UNCONN 0      0                127.0.0.1:323         0.0.0.0:*
udp   UNCONN 0      0                    [::1]:323            [::]:*
tcp   LISTEN 0      4096         127.0.0.53%lo:53          0.0.0.0:*
tcp   LISTEN 0      4096            127.0.0.54:53          0.0.0.0:*
tcp   LISTEN 0      4096               0.0.0.0:22          0.0.0.0:*
tcp   LISTEN 0      4096                  [::]:22             [::]:*

➡️Observation : The SSH daemon is in a LISTEN state on port 22 for both IPv4 (0.0.0.0) and IPv6 ([::]), confirming the service is reachable.

👉ping -c 3 localhost
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.015 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.026 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.111 ms

--- localhost ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2034ms
rtt min/avg/max/mdev = 0.015/0.050/0.111/0.042 ms

➡️The curl telnet test confirms Port 22 is open and the OpenSSH banner is visible, proving the service is responding.

👉sudo journalctl -u ssh -n 50

➡️Observation: Confirmed "Server listening on 0.0.0.0 port 22"; the service has started successfully and is awaiting connections.

👉sudo tail -n 50 /var/log/auth.log

➡️Observation: Verified successful login events; no evidence of widespread "Failed password" attempts which would indicate a brute-force attack.

==================If This Worsens (Next Steps)==================

👉Restart & Syntax Strategy

Before restarting, run sudo sshd -t to ensure the configuration is valid.

next : sudo systemctl restart ssh

👉Expanded Log Analysis (Deep Audit)

sudo journalctl -u ssh -n 200

👉Firewall & Security Group Verification







