🛠 The Core Components of Linux
1. Kernel
The kernel is the core, privileged component of an operating system that acts as the primary bridge between software applications and physical hardware.

Key Functions:

Allocates CPU time & RAM to running applications.

Uses drivers (devices) to communicate with hardware like printers and monitors.

Tracks which parts of memory are in use.

Operates in "kernel mode."

2. Userspace
Restricted, protected area of memory where user applications like web browsers, media players, and text editors run.

🔄 init / systemd
It is the traditional first process that starts when the system boots, while systemd is a modern replacement.

Every other process is a child of init.

Referred to as the "Mother of all processes."

📑 Process
Creation
The OS handles process creation by allocating necessary resources and assigning each process a unique identifier.

The Process Flow:

fork(): Creates a child process from the parent.

exec(): The child executes its specific task.

exit(): The child process finishes.

wait(): The parent waits for the child to finish.

resume: The parent continues its task.

How OS Manages Processes
The OS keeps track of the list of running processes.

Each process gets an entry in this list called a Process Control Block (PCB).

PCB includes metadata like:

Process ID (PID)

Information about the user who owns the process

Current state of the process

🚀 Systemd
Systemd is a modern system and service manager for Linux operating systems.

It acts as a "mother of all processes."

Responsible for bringing up the system after the kernel loads, managing user space services, and handling system resources, logging, and configuration
