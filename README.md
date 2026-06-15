# Linux Task 03: Process Management, System Monitoring & Basic Shell Scripting

**Date:** June 14, 2026  
**Name:** Hansini Kulal

**Task Objective:** To understand how the Linux operating system manages processes, monitors core system resources, and automates administrative tasks using shell scripts. These skills are essential foundational pillars for Linux Administrators, SOC Analysts, and Cyber Security Professionals.

---

## Part A: Process Monitoring

### 1. Conceptual Framework
* **What is a Process?** A process is an active, executing instance of a computer program. When an executable file or command is run, the Linux kernel loads its code into memory, allocates a chunk of system resources (such as CPU time, RAM space, and open file descriptors), and tracks its execution state.
* **What is a PID?** **PID** stands for **Process ID**. It is a unique, positive integer assigned by the Linux kernel to every process upon creation. The system uses this identifier to track status, manage resource allocations, and route control signals to that specific process throughout its lifecycle.

### 2. Command Evaluation & System Metrics
After running the `ps`, `ps aux`, `top`, and `htop` commands, the following resource anomalies were identified on the local system:
* **Process consuming the most CPU:** `[e.g., Firefox / Xorg / No. 1 process from your top command]`
* **Process consuming the most Memory:** `[e.g., Web Content / GNOME Shell]`

### 3. Verification Screenshots
<img width="1708" height="865" alt="Screenshot 2026-06-15 100814" src="https://github.com/user-attachments/assets/8b71112f-9e97-4f6c-bb1b-7d1ce972bb28" />



## Part B: Process Management

### 1. Process Lifecycle and Termination Workflow
To demonstrate interactive process control, a long-running background process was initialized, identified, and managed:

1. Initiate the Target Process: A separate terminal window was opened to launch a standalone command that pauses execution for 300 seconds:

   sleep 300

2. Isolate the Running Process: Within the primary control window, a process status query was executed and filtered using grep to find the specific Process ID (PID):

   ps aux | grep sleep

3. Graceful vs. Forced Termination: The process was managed using distinct termination control signals via the kill utility:
   * kill PID (SIGTERM / Signal 15): Requests the process to clean up its open file descriptors and state before exiting gracefully.
   * kill -9 PID (SIGKILL / Signal 9): Forces the Linux kernel to instantly drop the process from memory, preventing it from ignoring or blocking the termination request.

---

### 2. Execution Documentation & Metadata
The following tracking data reflects the execution parameters captured during the live exercise:

* PID Found: [e.g., 4321]
* Command Used: ps aux | grep sleep and kill [PID]
* Result: The kernel intercepted the kill execution payload, terminating the targeted sleep 300 worker thread. A subsequent check verified that the process was cleared from the active job table.

---

### 3. Verification Screenshots
Tracking, analyzing, and executing a process termination sequence:
<img width="1716" height="406" alt="Screenshot 2026-06-15 101213" src="https://github.com/user-attachments/assets/33ba34b6-9762-4aeb-9fdd-0d0f5cf50f4e" />

## Part C: System Monitoring

### 1. System Resource Metrics
Executing the system monitoring commands (free -h, df -h, uptime, and uname -a) yielded the following technical baseline for the machine:

| Metric | System Value |
| :--- | :--- |
| **Total RAM** | [e.g., 7.7Gi] |
| **Available RAM** | [e.g., 4.1Gi] |
| **Disk Usage (Root /)** | [e.g., 45% (22G used of 50G)] |
| **System Uptime** | [e.g., up 2 hours, 15 minutes] |
| **Kernel Version** | [e.g., Linux kali 6.1.0-amd64] |

---

### 2. Verification Screenshots
System hardware, disk usage, and kernel resource outputs:
<img width="1717" height="607" alt="Screenshot 2026-06-15 101406" src="https://github.com/user-attachments/assets/78f45113-5a33-4a98-b7ad-d4558167fe13" />

## Part D: Service Monitoring

### 1. Conceptual Framework
* **What is a Service?** A service (commonly referred to as a daemon) is a specialized utility or program that runs continuously in the background of the operating system without direct user intervention to manage specific system, hardware, or network functions.
* **Why are services important?** Services form the structural core of operating system tasks and infrastructure dependencies.
* They ensure critical system capacities—such as processing secure remote administrative logins (ssh) or dynamically routing connectivity interfaces (NetworkManager)—remain active, initialized, and highly responsive to incoming requests at all times.
* **How can a stopped service affect a system?** When a critical service goes down or is terminated, its corresponding system capability fails instantly. For example, stopping the ssh daemon immediately terminates existing remote terminal connections and blocks any new administrative access, while shutting down NetworkManager strips the host machine of its network routing and local internet interface capabilities.

---

### 2. Service Status Profiles Evaluated
* **Service 1:** ssh — Status: [active (running) / inactive (dead)]
* **Service 2:** NetworkManager — Status: [active (running) / inactive (dead)]

---

### 3. Verification Screenshots
Tracking, verifying, and analyzing active systemctl daemon target states:
<img width="1714" height="739" alt="Screenshot 2026-06-15 101539" src="https://github.com/user-attachments/assets/8a61bf27-de80-48e7-9c6d-4a61c6d827e5" />

## Part E: Shell Scripting

### 1. Script Implementation (system_report.sh)
This custom Bash script automates local host profiling and outputs a formatted system information report:

# Script Name: system_report.sh
# Purpose: Generates a basic system summary report for local system monitoring.
<img width="1717" height="877" alt="Screenshot 2026-06-15 102626" src="https://github.com/user-attachments/assets/2ec80441-d9c7-4759-a3b5-03ba5ada42a2" />


### 2. Execution Guide & Core Command Breakdown
To generate the report, the automated workflow processes 4 core system utilities sequentially inside the script framework:

* **whoami / hostname / pwd / date:** These foundational diagnostic utilities pull host variables directly from the shell session profile to document user context, local network machine aliases, directory paths, and runtime logging timestamps.
* **free -h:** Queries the virtual memory subsystem profile to output system memory footprints. The `-h` (human-readable) flag formats binary byte allocations into intuitive Gigabyte (Gi) and Megabyte (Mi) performance baselines.
* **df -h /** Queries the storage block devices table to show remaining drive infrastructure capacities. Specifying the `/` argument limits storage isolation directly to the root partition lifecycle.
* **awk 'NR==1 || NR==2'** Intercepts the raw data streams passing out of the disk space utility. It applies an operational parsing rule that drops unneeded device records, isolating only the headers (`NR==1`) and primary storage volume layouts (`NR==2`) to keep the report output perfectly clean.

---

### 3. Verification Screenshots
Executing the automated reporting script and verifying output metrics:
<img width="1713" height="523" alt="Screenshot 2026-06-15 103432" src="https://github.com/user-attachments/assets/6c6449ac-add3-4cb2-9e9e-cd00a3fa6f16" />

## Part F: Security Monitoring Challenge

Detailed analysis of network and user tracking commands utilized by security professionals:

### 1. netstat
* **Purpose:** Displays active network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.
* **Example Output:**
  tcp        0      0 0.0.0.0:22              0.0.0.0:* LISTEN      842/sshd
* **Security Use Case:** Essential for mapping out open/listening ports on a system and detecting unauthorized inbound or outbound persistent connections established by malicious software.

### 2. ss
* **Purpose:** A modern, faster utility designed to replace netstat. It dumps socket statistics and offers detailed network socket information by querying the kernel directly.
* **Example Output:**
  Netid  State      Recv-Q Send-Q Local Address:Port  Peer Address:Port
  tcp    LISTEN     0      128    0.0.0.0:22          0.0.0.0:*
* **Security Use Case:** Used in live incident response to quickly track active, malicious network links or active backdoors with significantly lower system performance overhead than netstat.

### 3. who
* **Purpose:** Displays a clean, simple list of all users currently logged into the local system, along with the terminal line they are using and their login time.
* **Example Output:**
  student  tty7         2026-06-14 09:12 (:0)
* **Security Use Case:** Helps security personnel check for unexpected administrative logins or unauthorized active terminal connections.

### 4. w
* **Purpose:** Provides a detailed overview of who is logged into the machine, what terminal they occupy, their remote IP address, and an active look at the exact command or program they are currently running.
* **Example Output:**
  09:15:22 up 2:03,  1 user,  load average: 0.12, 0.08, 0.02
  USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
  student  pts/0    192.168.1.50     09:13    1.00s  0.04s  0.01s w
* **Security Use Case:** Vital for tracking insider threats or identifying compromised accounts. If an unauthorized IP or standard user account is executing privileged commands, a SOC analyst will instantly see it under the WHAT column.

### 5. last
* **Purpose:** Reads backward through the system log file /var/log/wtmp to display a historical listing of all user logins, logouts, boot events, and runlevel switches since the log file was created.
* **Example Output:**
  student  pts/0        192.168.1.50     Sun Jun 14 09:13   still logged in
  reboot   system boot  6.1.0-amd64      Sun Jun 14 07:12   running
* **Security Use Case:** Used during forensic post-mortem analysis to determine the precise timeframe an attacker gained access to a compromised system, what IP they pivoted from, and whether they forced an unclean system reboot to hide their tracks.
## Part G: Mini SOC Activity

As a **Security Analyst** handling an escalation involving a severely degraded, slow-running system, here is my methodical approach to threat hunting, identification, and containment:

### 1. Identifying Resource-Heavy Processes
To isolate the performance bottleneck, I would utilize active process monitoring interfaces like top or htop[span_0](start_span)[span_0](end_span). Once inside top, I would press the P key to dynamically sort all active system tasks by raw CPU utilization percentage, or press the M key to sort them by physical memory consumption. Alternatively, to obtain an immediate command-line snapshot, I could run a sorted process status query:

ps aux --sort=-%cpu | head -n 10

This instantly isolates the top 10 most demanding worker threads running on the system infrastructure.

### 2. Determining if a Process is Suspicious
Once the resource-heavy processes are isolated, I would evaluate whether they represent a security anomaly using several criteria:
* **Binary Path Verification:** I would inspect the underlying execution path by checking the process link in the proc directory using the command `ls -l /proc/[PID]/exe`. If a critical system utility like sshd, kworker, or systemd is executing out of a temporary or unprivileged directory such as /tmp, /var/tmp, or /dev/shm, it strongly indicates a malicious binary masquerading as a system process.
* **User and Privilege Context:** I would review which account initiated the command. A high-resource system utility running under the context of an unprivileged web application user account like www-data, apache, or nobody is an immediate red flag.
* **Parent-Child Tree Traversal:** I would map the process ancestry using a tool like `pstree` or running `ps -efj`. For example, if a standard web server parent process spawns an interactive command shell like /bin/bash or /bin/sh, it heavily implies an attacker has successfully exploited a web-shell backdoor.
* **Network Socket Affiliation:** I would check whether the specific process has established active network connections by running `ss -tapu | grep [PID]`. An unknown binary maintaining persistent connections to unverified external IP addresses points directly to a command-and-control (C2) beacon or a data exfiltration script.

### 3. Forensic Information Collected Before Termination
Before issuing a termination signal and destroying volatile memory indicators, I would preserve the following forensic details to ensure full security visibility and support downstream incident response tracking:
* **Full Process Metadata:** I would record the exact command-line arguments, flags, start time, and lineage using the command `ps -fp [PID]`.
* **Network Connection State:** I would log all active network sockets and remote endpoints attached to the process using `ss -mpe -p` or `netstat -anp`.
* **Open File Handles:** I would execute `lsof -p [PID]` to document exactly what system files, open network descriptors, temporary scripts, and shared object libraries the binary is interacting with on the disk.
* **Environment Block Inspection:** I would review the environmental variables passed to the process by running `cat /proc/[PID]/environ`. This frequently reveals hidden directories, hardcoded configuration data, or attacker-controlled parameters.
