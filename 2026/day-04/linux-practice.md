1. Process Checks

top : This is like the Windows Task Manager. It shows me which programs are eating up my CPU and RAM in real-time.
ps aux | grep nginx : This is a "search" command. ps aux lists every single thing running, and grep filters it so I only see the Nginx stuff.

2. Service Checks
systemctl status docker : shows if it is running or not
sudo systemctl stop nginx

3. Log Checks
journalctl -u ssh --since "1 hour ago" : tells who has tried to access this machine in ssh
tail -n 20 /var/log/syslog : tails means "show me the bottom"

Troubleshooting steps
systemctls status nginx (verify)
journalctl -xe(figure out the specific error)
sudo systemctl restart nginx(fresh start)
