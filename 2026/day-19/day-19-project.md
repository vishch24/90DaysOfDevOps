# Day 19 – Shell Scripting Project: Log Rotation, Backup & Crontab

### Task 1: Log Rotation Script
Create `log_rotate.sh` that:
1. Takes a log directory as an argument (e.g., `/var/log/myapp`)
2. Compresses `.log` files older than 7 days using `gzip`
3. Deletes `.gz` files older than 30 days
4. Prints how many files were compressed and deleted
5. Exits with an error if the directory doesn't exist

```bash
#!/bin/bash

LOG_DIR="$1"

# 1. Takes a log directory as an argument (e.g., `/var/log/myapp`)
if [ -z "$LOG_DIR" ]; then
    echo "Error: Please specify a log directory." >&2
    exit 1
fi

# 5. Exits with an error if the directory doesn't exist
if [ ! -d "$LOG_DIR" ]; then
    echo "Error: Directory '$LOG_DIR' does not exist." >&2
    exit 1
fi

# 4. Count files before modifying them
COMPRESSED_COUNT=$(find "$LOG_DIR" -type f -name "*.log" -mtime +7 | wc -l)
DELETED_COUNT=$(find "$LOG_DIR" -type f -name "*.gz" -mtime +30 | wc -l)

# 2. Compresses `.log` files older than 7 days using `gzip`
find "$LOG_DIR" -type f -name "*.log" -mtime +7 -exec gzip {} \;

# 3. Deletes `.gz` files older than 30 days
find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -exec rm {} \;

# 4. Prints how many files were compressed and deleted
echo "Log rotation complete."
echo "Files compressed: $COMPRESSED_COUNT"
echo "Files deleted:    $DELETED_COUNT"
```
Output:
```text
Log rotation complete.
Files compressed: 3
Files deleted:    2
```

---

### Task 2: Server Backup Script
Create `backup.sh` that:
1. Takes a source directory and backup destination as arguments
2. Creates a timestamped `.tar.gz` archive (e.g., `backup-2026-02-08.tar.gz`)
3. Verifies the archive was created successfully
4. Prints archive name and size
5. Deletes backups older than 14 days from the destination
6. Handles errors — exit if source doesn't exist

```bash
#!/bin/bash

# 1. Takes a source directory and backup destination as arguments
SOURCE_DIR="$1"
DESTINATION_DIR="$2"
TIMESTAMP=$(date +"%Y-%m-%d")
BACKUP_NAME="backup-${TIMESTAMP}.tar.gz"

# 6. Handle errors — exit if arguments are missing or source doesn't exist
if [ -z "$SOURCE_DIR" ] || [ -z "$DESTINATION_DIR" ]; then
    echo "Error: Missing arguments." >&2
    echo "Usage: $0 /path/to/source /path/to/destination" >&2
    exit 1
fi

if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory '$SOURCE_DIR' does not exist." >&2
    exit 1
fi

# Ensure destination directory exists, create it if it doesn't
mkdir -p "$DESTINATION_DIR"

echo "Starting backup of $SOURCE_DIR..."

# 2. Creates a timestamped `.tar.gz` archive (e.g., `backup-2026-02-08.tar.gz`)
tar -czf "$DESTINATION_DIR/$BACKUP_NAME" -C "$SOURCE_DIR" .

# 3. Verifies the archive was created successfully
if [ $? -eq 0 ]; then
    echo "Backup created successfully."

    # 4. Prints archive name and size
    echo "Archive Name: $BACKUP_NAME"
    echo "Archive Size: $(du -sh "$DESTINATION_DIR/$BACKUP_NAME" | cut -f1)"
else
    echo "Error: Backup creation failed." >&2
    exit 1
fi

# 5. Deletes backups older than 14 days from the destination
echo "Cleaning up backups older than 14 days..."
find "$DESTINATION_DIR" -type f -name "backup-*.tar.gz" -mtime +14 -exec rm {} \;

echo "Backup process finished."
```
Output:
```text
Starting backup of ../devops...
Backup created successfully.
Archive Name: backup-2026-06-26.tar.gz
Archive Size: 12K
Cleaning up backups older than 14 days...
Backup process finished.
vishakha@Vishakha:~/shell-scripting$ cd ../devops_backups && ls backup-*
backup-2026-06-26.tar.gz
```

---

### Task 3: Crontab
1. Read: `crontab -l` — what's currently scheduled?

   Output:
   ```text
   no crontab for vishakha
   ```
2. Understand cron syntax:
   ```
   * * * * *  command
   │ │ │ │ │
   │ │ │ │ └── Day of week (0-7)
   │ │ │ └──── Month (1-12)
   │ │ └────── Day of month (1-31)
   │ └──────── Hour (0-23)
   └────────── Minute (0-59)
   ```
3. Write cron entries (in your markdown, don't apply if unsure) for:
   - Run `log_rotate.sh` every day at 2 AM
   - Run `backup.sh` every Sunday at 3 AM
   - Run a health check script every 5 minutes
   ```
   # Run log_rotate.sh every day at 2 AM
   0 2 * * * /home/vishakha/shell-scripting/log_rotate.sh /var/log/myapp >> /home/vishakha/log_rotate.log 2>&1

   # Run backup.sh every Sunday at 3 AM
   0 3 * * 0 /home/vishakha/shell-scripting/backup.sh /var/log/myapp /home/vishakha/myapp_backups >> /home/vishakha/myapp_backup.log 2>&1

   # Run a health check script every 5 minutes
   */5 * * * * /home/vishakha/shell-scripting/disk_check.sh >> /home/vishakha/health_check.log 2>&1
   ```
   Output:
   ```text
   vishakha@Vishakha:~/shell-scripting$ cat /home/vishakha/log_rotate.log
   Log rotation complete.
   Files compressed: 0
   Files deleted:    0
   Log rotation complete.
   Files compressed: 0
   Files deleted:    0
   Log rotation complete.
   Files compressed: 0
   Files deleted:    0
   vishakha@Vishakha:~/shell-scripting$ cat /home/vishakha/health_check.log
   ==========================================
            SYSTEM HEALTH CHECK
   ==========================================

   === Root Filesystem Usage (/) ===
   Filesystem      Size  Used Avail Use% Mounted on
   /dev/sdd       1007G   13G  944G   2% /

   === System Memory Usage ===
                  total        used        free      shared  buff/cache   available
   Mem:           3.7Gi       465Mi       3.0Gi       3.5Mi       310Mi       3.2Gi
   Swap:          1.0Gi          0B       1.0Gi

   ==========================================
   vishakha@Vishakha:~/shell-scripting$ cat /home/vishakha/myapp_backup.log
   Starting backup of /var/log/myapp...
   Backup created successfully.
   Archive Name: backup-2026-06-26.tar.gz
   Archive Size: 4.0K
   Cleaning up backups older than 14 days...
   Backup process finished.
   Starting backup of /var/log/myapp...
   Backup created successfully.
   ```

---

### Task 4: Combine — Scheduled Maintenance Script
Create `maintenance.sh` that:
1. Calls your log rotation function
2. Calls your backup function
3. Logs all output to `/var/log/maintenance.log` with timestamps
4. Write the cron entry to run it daily at 1 AM

`crontab -e`:
```
0 1 * * * /home/vishakha/shell-scripting/maintenance.sh
```

```bash
#!/bin/bash

# Configuration
LOG_FILE="/home/vishakha/maintenance.log"
LOG_DIR="/var/log/myapp"
BACKUP_DESTINATION="/home/vishakha/myapp_backups"

# Scripts paths
LOG_ROTATE_SCRIPT="/home/vishakha/shell-scripting/log_rotate.sh"
BACKUP_SCRIPT="/home/vishakha/shell-scripting/backup.sh"

# Function to print messages with system timestamps
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> "$LOG_FILE" 2>&1
}

# Start maintenance process
log_message "START: System maintenance process triggered."

# 1. Call log rotation script
if [ -x "$LOG_ROTATE_SCRIPT" ]; then
    log_message "RUNNING: Log rotation starting..."
    # Capture the output of log_rotate.sh into our maintenance log
    "$LOG_ROTATE_SCRIPT" "$LOG_DIR" >> "$LOG_FILE" 2>&1
    log_message "SUCCESS: Log rotation finished."
else
    log_message "ERROR: Log rotation script not found or not executable at $LOG_ROTATE_SCRIPT"
fi

# 2. Call backup script
if [ -x "$BACKUP_SCRIPT" ]; then
    log_message "RUNNING: System backup starting..."
    # Capture the output of backup.sh into our maintenance log
    "$BACKUP_SCRIPT" "$LOG_DIR" "$BACKUP_DESTINATION" >> "$LOG_FILE" 2>&1
    log_message "SUCCESS: System backup finished."
else
    log_message "ERROR: Backup script not found or not executable at $BACKUP_SCRIPT"
fi

log_message "END: System maintenance process completed."

```
Output:
```text
vishakha@Vishakha:~/shell-scripting$ cat /home/vishakha/maintenance.log
2026-06-26 14:47:01 - START: System maintenance process triggered.
2026-06-26 14:47:01 - RUNNING: Log rotation starting...
Log rotation complete.
Files compressed: 0
Files deleted:    0
2026-06-26 14:47:01 - SUCCESS: Log rotation finished.
2026-06-26 14:47:01 - RUNNING: System backup starting...
Starting backup of /var/log/myapp...
Backup created successfully.
Archive Name: backup-2026-06-26.tar.gz
Archive Size: 4.0K
Cleaning up backups older than 14 days...
Backup process finished.
```

---

### What I learned

- Cron jobs use a five-field syntax (minute, hour, day, month, day of week) to execute scripts automatically at specific intervals.
- Cron runs with a minimal environment, always use absolute paths (e.g., /home/ubuntu/) for all scripts and file destinations.
- Redirecting both standard output and errors (using `>> logfile 2>&1`) is critical for auditing background tasks that have no visible terminal.
