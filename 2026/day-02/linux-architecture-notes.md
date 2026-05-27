
# Day 02 – Linux Architecture, Processes, and systemd

## What is Linux?
- Linux is an operating system which is lightweight, stable and secure which follows file system hierarchy. It is a free and open-source platform to run commands on any system using flavours like Ubuntu, RedHat, CentOS, etc. It can also be called as a kernel which is the heart of the operating system. More than 80% of the systems use Linux.

---

## Linux Architecture:

<img width="400" height="auto" alt="image" src="https://github.com/user-attachments/assets/813395d7-5b2c-4116-b0b0-8f4bbff6829d" />

## What are the core components of Linux?
- **Applications**: which are also known as software applications such as web browsers, games, office suites, etc.
- **Shell**: is a command line interface (CLI) which helps users to interact with the system. E.g: bash, ssh, zsh.
- **Kernel**: is the heart of the system which holds the code written in C programming.

`Everything in Linux is a process.`

`It starts with PID 1 i.e. systemd, where 'd' represents 'Daemon' (aka background process).`

---

## What systemd does and why it matters?
- `systemd` is usually the start of the process i.e. `PID 1` on boot.
- It is the foundation system and service manager for Linux which handles overall background process, monitoring and ensures your operating system runs smoothly and efficiently.
- Previously Linux relied on older `init` systems such as SysVinit which made the processes slower because of running services sequentially.
- Now, `systemd` runs multiples processes in parallel ensuring faster booting time.
- It ensures services start, stop, and restart properly. If a service crashes, `systemd` autmatically detect and restarts it so the applications run smoothly.
- It uses the command-line tool `systemctl` to control the background processes, system resources including the overall state of the operating system on modern distributions like Ubuntu, Fedora and CentOS, etc.

### For Example: `How your laptop starts?`

<div align="center">

`POWER ON` \
⬇️ \
`MOTHERBOARD - (BIOS) BASIC INPUT OUTPUT SYSTEM [BIOS AKA FIRMWARE]` \
⬇️ \
`BOOTLOADER (lINUX KERNEL KA CODE KAHA RAKHA HAI)` \
⬇️ \
`KERNEL (STARTS A PROCESS)` \
⬇️ \
`SYSTEMD (PID) PROCESS ID 1` \
⬇️ \
`DOCKER/KUBERNETES/SSH/NGINX/PRINTER/SCANNER`

</div>

---

## File System Hierarchy:
- `Everything in Linux starts from '/'.`
- `Everything in Linux is a file or directory.`
- `Everything in Linux is a process.`
- `'/' is a root directory (aka folder).`

---

## Shell commands for practice:
- `pwd`: Present Working Directory or Print Working Directory.
- `cd`:  Change directory.
- `ls`: List files or directory. (d = directory, l = Link)
- `cat etc/os-release`: Linux distribution and version.
- `echo`: Displays text on the interface.  

---

## How to run Linux in Docker?
- `docker run -itd ubuntu`
- `docker exec -it <first 5 chars of token generated from the above command> bash`





