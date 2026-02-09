Linux Command Toolkit


Process Management
top: Displays real-time view of system processes.

ps aux: Lists every running process on the system with user and memory details.

kill -9 [PID]: Forcefully terminates a process using its Process ID.

systemctl: Checks whether background services (like Nginx) are active or not.

lsof -i :80: "List of Open Files"—shows which process is using a specific port.

File System
pwd: Shows the full path of your current directory.

ls -la: Lists all files, including hidden ones.

cd ..: Moves one level up in the directory tree.

mv [old] [new]: Renames a file or moves it to a different location.

df -h: Displays disk space usage in human-readable format (GB/MB).

du -sh: Shows the total size of the current directory.

chmod 400 [file]: Sets restrictive permissions (Standard for AWS .pem keys).

Networking Troubleshooting
ip addr show: Displays all network interfaces and their assigned IP addresses.

ping [host]: Checks if a remote server is reachable.

curl -I [URL]: Fetches only the HTTP header (Great for testing Nginx response).

netstat -tuln: Lists all listening ports to verify services are bound correctly.

telnet [IP] [PORT]: Tests if a specific port on a remote server is open.

nslookup [domain]: Queries DNS to see which IP address a domain name points to.

tail -f /var/log/nginx/access.log: Follows web server logs in real-time to watch traffic.
