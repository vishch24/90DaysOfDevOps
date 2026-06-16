# Day 12 – Breather & Revision (Days 01–11)

## Mindset & plan
- I have come here so far with the knowledge of linux commands, process management, service management, users and groups, and file management. On day 1, I was at the surface of my DevOps journey. Now, I'll do hands-on practice from Day 1-11.

## Processes & services
- `ps`:- It showed the current running processes.

  <img width="608" height="309" alt="image" src="https://github.com/user-attachments/assets/4a49a3c5-8bfa-49cb-801f-d3d0617754f0" />

- `systemctl status`:- It showed the current system live status such as services running, background processes, etc.  

  <img width="940" height="577" alt="image" src="https://github.com/user-attachments/assets/2f387e39-fac4-4953-a3fe-ec677f28c1b6" />

- `journalctl -u cron`:- It filters logs for cron service.  

  <img width="940" height="499" alt="image" src="https://github.com/user-attachments/assets/baf4932a-b39e-41a9-81e0-74e5d3d04da5" />


## File skills
- `ls -l`:- Long list of files or directories.
- `mkdir`:- Create a new directory.
- `cd`:- Change directory.
- `touch`:- Create a new file.
- `vim <filename>`:- Open a file in vim editor.
- `cat <filename>`:- Shortcut for concatenate. Displays content of the file.
- `cp <source> <destination>`:- Copy a file from source to the destination path.
- `echo "Some text" > <filename>`:- Add text to a file.
- `echo "Some other text" >> <filename>`:- Append text to a file.
- `chmod`:- Change permissions or mode of a file or directory.
- `chown`:- Change owner of a file or directory.

## Cheat sheet refresh
Commands I’d reach for first in an incident.
- `pwd`:- Present Working Directory or Print Working Directory.
- `ls -l`:- Long list of files or directories such as date, permissions, user, group, other users, etc.
- `ps, top, htop`:- Background running processes.
- `systemctl status <service_name>`:- Live system status such as services running, background processes, etc.
- `journalctl -u <service_name>`:- Logs for a specific service.

## User/group sanity
```
Scenario: Setting Up a Dev Team on a New Server.
You're a sysadmin. A small dev team just joined. Your job is to set up their users, a shared project directory, some config files, and lock down who can access what.
```
### Step 1 — Create users and groups
```bash
sudo useradd -m alice
sudo useradd -m bob
sudo useradd -m charlie

sudo groupadd developers
sudo groupadd leads
```
`alice` is the team lead. `bob` and `charlie` are developers.
```bash
sudo usermod -aG developers bob
sudo usermod -aG developers charlie
sudo usermod -aG developers,leads alice
```
Or add both developers at once:
```bash
sudo gpasswd -M bob,charlie developers
```

### Step 2 — Create the project directory structure
```bash
sudo mkdir -p /opt/webproject/scripts
sudo mkdir -p /opt/webproject/config
sudo mkdir -p /opt/webproject/logs
```
`mkdir -p` creates nested dirs in one shot.

### Step 3 — Create some files
```bash
sudo touch /opt/webproject/config/app.conf
cat > /opt/webproject/logs/deploy.log
```
Write a deploy script:
```bash
sudo vim /opt/webproject/scripts/deploy.sh
```
Inside the file:
```bash
#!/bin/bash
echo "Deploying app..."
```

### Step 4 — Set ownership
The whole project belongs to `alice` (team lead), group `developers`:
```bash
sudo chown -R alice:developers /opt/webproject
```
Check it recursively:
```bash
ls -lR /opt/webproject
```
Change only the group of the `logs/` directory to `leads` (only leads should manage logs):
```bash
sudo chgrp leads /opt/webproject/logs
```
Or using the shorthand:
```bash
sudo chown alice:leads /opt/webproject/logs
```

### Step 5 — Set permissions
The main project dir — readable and writable by owner and group, others can't touch it:
```bash
sudo chmod 775 /opt/webproject
```
The config file — owner can read/write, group can only read, others get nothing:
```bash
sudo chmod 640 /opt/webproject/config/app.conf
```
The deploy script — needs to be executable:
```bash
sudo chmod +x /opt/webproject/scripts/deploy.sh
```
Lock the deploy log — no one should write to it directly:
```bash
sudo chmod -w /opt/webproject/logs/deploy.log
```

### Step 6 — Verify and test
Read permissions:
```bash
ls -l /opt/webproject/scripts/deploy.sh
ls -l /opt/webproject/config/app.conf
```
Check first 5 lines of the log:
```bash
head -n 5 /opt/webproject/logs/deploy.log
```
Test that `bob` can create a file in the project dir (he's in `developers`, dir is `775`):
```bash
sudo -u bob touch /opt/webproject/bob-test.txt
```
Test that `bob` cannot write to the config file (it's `640`, group has only read):
```bash
sudo -u bob bash -c "echo test >> /opt/webproject/config/app.conf"
# Expected: Permission denied
```

## 1. Which 3 commands save you the most time right now, and why?
```bash
sudo gpasswd -M bob,charlie developers
```
**Why**:- Add multiple users to the group.

```bash
sudo mkdir -p /opt/webproject/scripts
```
**Why**:- Create new recurring/nested directories.

```bash
sudo chown alice:leads /opt/webproject/logs
```
**Why**:- Change owner and group of a file in one shot.

## 2. How do you check if a service is healthy? List the exact 2–3 commands you’d run first.
- `systemctl status <service>`:- Current status of a service.
- `journalctl -u <service>`:- Logs of a service.
- `systemctl --failed`:- List of all failed services on the machine.

## 3. How do you safely change ownership and permissions without breaking access? Give one example command.
```bash
sudo chown alice:leads /opt/webproject/logs
```

## 4. What will you focus on improving in the next 3 days?
- User management
- File ownership
- Processes and services.
