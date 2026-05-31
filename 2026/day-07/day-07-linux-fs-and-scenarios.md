# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

## Part 1: Linux File System Hierarchy (30 minutes)

**Core Directories (Must Know):**
- `/` (root) - The starting point of everything

It includes directories like `dev` for devices, `home` for users, `etc` for managing users, system state and booting, downloading software packages, and networking.

After running, `ls -l /`, we get:
```bash
total 1048656
lrwxrwxrwx   1 root root          7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root       4096 Feb 26  2024 bin.usr-is-merged
drwxr-xr-x   5 root root       4096 May 16 19:58 boot
drwxr-xr-x  17 root root       3820 May 30 14:27 dev
drwxr-xr-x 118 root root       4096 May 30 14:27 etc
drwxr-xr-x   3 root root       4096 Feb 10  2025 home
...
...
```
I would use this when I want to switch from directories.

---

- `/home` - User home directories

It includes different user's directories for example, `ubuntu`.

After running, `ls -l /home/`, we get:
```bash
total 4
drwxr-x--- 3 ubuntu ubuntu 4096 Feb 10  2025 ubuntu
```
I would use this when creating new users and managing them.

---

- `/root` - Root user's home directory

It includes the root user's home directory which is not accessible by other users.

After running, `ls -l /root/`, we get:
```bash
total 0
lrwxrwxrwx 1 root root 1 May 16 19:59 filesystem -> /
```
I would use this when switching user.

---

- `/etc` - Configuration files

It contains configuration file directories like `apt` for installing software packages, `passwd` for adding user's passwords while creating users, `opt` for optional software packages.

After running, `ls -l /etc/`, we get:
```bash
total 988
drwxr-xr-x 4 root root       4096 Jan 22  2025 ModemManager
drwxr-xr-x 3 root root       4096 May 16 19:58 NetworkManager
drwxr-xr-x 2 root root       4096 May 16 19:58 PackageKit
drwxr-xr-x 4 root root       4096 Jan 22  2025 X11
-rw-r--r-- 1 root root       3444 Jul  5  2023 adduser.conf
drwxr-xr-x 2 root root       4096 May 16 19:59 alternatives
drwxr-xr-x 3 root root       4096 May 16 19:59 apache2
...
...
```
I would use this when managing users, system state and booting, downloading software packages, and networking.

---

- `/var/log` - Log files (very important for DevOps!)

It includes log files like `syslog` which help in analysing system infrastructure failures, kernel-level errors, service lifecycle changes, and hardware resource bottlenecks; `sysstat` for CPU throttling, memory leaks, disk I/O latency, and network dropouts; `journal` for OS-level debugging, daemon failures, and service orchestration, etc.

After running, `ls -l /var/log/`, we get:
```bash
total 1360
drwxr-sr-x+ 3 root      systemd-journal   4096 Feb 10  2025 journal
-rw-r-----  1 syslog    adm              66327 May 30 14:27 kern.log
-rw-r-----  1 syslog    adm             143066 May 16 19:59 kern.log.1
drwxr-xr-x  2 root      root              4096 May 30 14:49 killercoda
drwxr-xr-x  2 landscape landscape         4096 Jan 22  2025 landscape
-rw-rw-r--  1 root      utmp               292 Feb 10  2025 lastlog
drwx------  2 root      root              4096 Feb 10  2025 private
-rw-r-----  1 syslog    adm             150958 May 30 15:40 syslog
-rw-r-----  1 syslog    adm             387880 May 16 20:00 syslog.1
drwxr-xr-x  2 root      root              4096 May 30 14:27 sysstat
drwxr-x---  2 root      adm               4096 Feb 10  2025 unattended-upgrades
-rw-rw-r--  1 root      utmp              9600 May 30 14:27 wtmp
...
...
```
I would use this when system troubleshooting, infrastructure monitoring, automated alerting, and security auditing.

---

- `/tmp` - Temporary files

It contains temporary files which automatically wipes away after reboot.

After running, `ls -l /tmp/`, we get:
```bash
total 36
drwxr-xr-x 2 root root 4096 May 30 14:27 github-remote
drwxr-xr-x 2 root root 4096 May 30 14:27 http-remote
drwx------ 3 root root 4096 May 30 14:27 systemd-private-a070a4eedb75412e859516cff594bda5-ModemManager.service-METbm8
...
...
```
I would use this when storing temporary, transient files.

---

**Additional Directories (Good to Know):**

- `/bin` - Essential command binaries

It includes command binaries which are everyday system tools to execute like, `mkdir` to create a new directory, `cp` to copy files and directories, `ls` to list files or directories, `cat` to concatenate and display file content.

After running, `ls -l /bin/`, we get:
```bash
total 178068
-rwxr-xr-x 1 root root      51648 Jan 23 10:51 '['
-rwxr-xr-x 1 root root      14720 Mar  6 16:10  addpart
lrwxrwxrwx 1 root root         26 Dec  3 15:08  addr2line -> x86_64-linux-gnu-addr2line
-rwxr-xr-x 1 root root      18824 Oct 22  2024  apt
-rwxr-xr-x 1 root root      84448 Oct 22  2024  apt-cache
-rwxr-xr-x 1 root root      27104 Oct 22  2024  apt-cdrom
-rwxr-xr-x 1 root root      27024 Oct 22  2024  apt-config
-rwxr-xr-x 1 root root      51680 Oct 22  2024  apt-get
-rwxr-xr-x 1 root root      28173 Oct 22  2024  apt-key
-rwxr-xr-x 1 root root      51680 Oct 22  2024  apt-mark
...
...
```
I would use this when running my everyday commands.

---

- `/usr/bin` - User command binaries

It includes non-essential user command binaries (executable programs) that are managed by the distribution's package manager like, `vim` as a modern and advanced text editor; `nano` as a editor for beginners; `python` to run python scripts, etc.

After running, `ls -l /usr/bin/`, we get:
```bash
total 178068
lrwxrwxrwx 1 root root         10 Aug  8  2024  python3 -> python3.10
lrwxrwxrwx 1 root root         17 Aug  8  2024  python3-config -> python3.10-config
-rwxr-xr-x 1 root root    5937704 Mar  3 11:56  python3.10
lrwxrwxrwx 1 root root         34 Mar  3 11:56  python3.10-config -> x86_64-linux-gnu-python3.10-config
lrwxrwxrwx 1 root root         20 May 13 05:57  vi -> /etc/alternatives/vi
lrwxrwxrwx 1 root root         22 May 13 05:57  view -> /etc/alternatives/view
lrwxrwxrwx 1 root root         21 May 13 05:57  vim -> /etc/alternatives/vim
-rwxr-xr-x 1 root root    3787888 May  6 16:56  vim.basic
lrwxrwxrwx 1 root root         25 May 13 05:57  vimdiff -> /etc/alternatives/vimdiff
-rwxr-xr-x 1 root root       2154 May  6 16:56  vimtutor
-rwxr-xr-x 1 root root      39160 Oct 31  2023  vmstat
...
...
```
I would use this when executing programs and text editors.

---

- `/opt` - Optional/third-party applications

It includes optional third-party applications which are not part of the default operating system installation like `chrome`, `discord`, `spotify`, etc.

After running, `ls -l /opt/`, we get:
```bash
total 16
drwx--x--x 4 root root 4096 May 16 19:58 containerd
drwxr-xr-x 2 root root 4096 May 30 17:49 kc-internal
-rw-r--r-- 1 root root    3 May 16 19:59 package.json
drwxr-xr-x 8 root root 4096 May 16 19:30 theia
...
...
```
I would use this when installing third-party applications whenever needed.

---

## Part 2: Scenario-Based Practice

### Scenario 1: Service Not Starting

```
A web application service called 'myapp' failed to start after a server reboot.
What commands would you run to diagnose the issue?
Write at least 4 commands in order.
```

**My Solution (Step by step):**

**Step 1:** Check service status
```bash
systemctl status myapp
```
**Why this command?** It shows if the service is active, failed, or stopped

**Step 2:** Check service is enabled
```bash
systemctl is-enabled myapp
```
**Why this command?** To check if the service is enabled.

**Step 3:** Check systemd-journald logs
```bash
journalctl -u myapp -n 50
```
**Why this command?** It shows if some errors or warning occurred in the log file of the service.

**Step 4:** Restart the service
```bash
systemctl restart myapp
```
**Why this command?** After solving the error, it will restart the service.
**What I learned**: Always check status first, then investigate based on what you see.

---

### Scenario 2: High CPU Usage

```
Your manager reports that the application server is slow.
You SSH into the server. What commands would you run to identify
which process is using high CPU?
```

**My Solution (Step by step):**

**Step 1:** Check CPU usage with `top`
```bash
top 
```
**Why this command?** It shows all running processes in real time. 

**Step 2:** Check CPU usage with `htop`
```bash
htop
```
**Why this command?** Same as `top` but, with better visuals to check CPU usage.

**Step 3:** Check CPU usage with `ps`
```bash
ps aux --sort=-%cpu | head -10
```
**Why this command?** It shows top 10 CPU-consuming processes at that exact moment sorted by CPU usage, not real-time.
**What I learned**: If CPU is overloaded, it can slow down the system.

---

### Scenario 3: 

```
A developer asks: "Where are the logs for the 'docker' service?"
The service is managed by systemd.
What commands would you use?
```

**My Solution (Step by step):**

**Step 1:** Check docker service status
```bash
systemctl status docker
```
**Why this command?** It shows if the docker service is active, failed, or enabled.

**Step 2:** Check docker service through systemd-journald
```bash
journalctl -u docker
```
**Why this command?** It shows log file for docker service.

**Step 3:** Check docker service status by units and number of lines
```bash
journalctl -u docker -n 50
```
**Why this command?** It uses flags like `-u` to filter the system logs and show entries only for a specific systemd unit and `-n` to limit number of lines to show the system logs generated specifically by the Docker background service.

**Step 4:** Check docker service status by units and follow logs in real-time
```bash
journalctl -u docker -f
```
**Why this command?** It uses flags like `-u` and `-f` flags to show 

**Step 5:** 
```bash

```
**Why this command?** 
**What I learned**: 

---

### Scenario 4: 

```

```

**My Solution (Step by step):**

**Step 1:** 
```bash

```
**Why this command?** 

**Step 2:** 
```bash

```
**Why this command?** 

**Step 3:** 
```bash

```
**Why this command?** 

**Step 4:** 
```bash

```
**Why this command?** 
**What I learned**: 

















