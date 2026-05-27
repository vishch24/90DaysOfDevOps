# Day 04 – Linux Practice: Processes and Services

## Process checks
- Most commonly used process commands are `ps` for snapshots and `top` and `htop` for real-time monitoring.
- Commands:
  - `ps aux`: Gives detailed information like CPU usage and memory usage, etc. of the processes running in the system.
  - `ps -ef`: Gives full-format listings of the running processes including Parent Process ID (PPID).
  - `ps -u [username]`: Filters only the specific user's processes.
  - `ps -p [PID]`: Shows details for a specific process ID.
  - `top`: Standard interactive monitor which lists the processes including CPU usage by default and updates every few seconds.
  - `htop`: Enhanced and colorful version of `top`.
  - `pgrep [name]`: Returns PID of the specified name.
  - `pidof [name]`: Finds PID of the specific running executable.
  - `ps -ef | grep [name]`: Find the process by searching its name from the full lists.

## Service checks
- In modern systems, `systemctl` is the primary command to check and manage services. For older systems like SysVinit, `service` command is used.
- Commands:
  - `systemctl status <service_name>` or `service <service_name> status`: To check if the specific service is running and view in its recent logs.
  - `systemctl list-units --type=service --state=active`: List all active services.
  - `systemctl list-unit-files --type=service`: Lists all installed services.
  - `service --status-all`: Lists everything.
    - `[ + ]`: Service is running.
    - `[ - ]`: Service is stopped.
  - `systemctl is-active <service_name>`: Checks if the service is active.
  - `systemctl is-enabled <service_name>`: Checks if the service is enabled.
  - `systemctl is-failed <service_name>`: Checks if the service is failed.

## Log checks
- `journalctl` command is mostly used to check logs in Linux. It manages logs from the `systemd` journal and allows extensive filtering by time, service or priority.
- Commands:
  - `journalctl -u <service_name>`: Filters logs for a specific service.
  - `journalctl -f`: Follows the logs in real-time as they are written.
  - `tail -f /var/log/syslog`: Continuously monitors the main system for new log entries.

## Mini troubleshooting steps
- Step 1: `ps aux` to check the real-time CPU and memory usage.
- Step 2: `ps -u vishakha` to check a vishakha's processes.
- Step 3: `systemctl list-unit-files --type=service` to list all the installed services.
- Step 4: `journalctl -u cron` to filter logs for cron service.
- Step 5: `kill -9 365` to kill a process. 
