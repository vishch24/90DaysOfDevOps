# Day 11 – File Ownership Challenge (chown & chgrp)

## Understanding Ownership
```bash
cd /home
ls -l
```

Output:
```bash
(`d` = directory, `-` = file, `l` = link)
⬆️
drwxr-x--- 3 ubuntu ubuntu 4096 Feb 10 2025 ubuntu
              ⬇️    ⬇️    ⬇️     ⬇️        ⬇️
             owner  group  size    date    file/directory
```

### What's the difference between owner and group?

| Owner | Group |
| :--- | :--- |
| Individual user who owns files.  | Collection of multiple users.  |
| Can modify files using `chmod` and change groups  | Members only inherit the read/write/execute rights assigned to the group.  |
| Modifies files using `chmod`  | Modifies files using `chmod` or `chgrp`  |

## Files & Directories Created
```
home/
├── app-logs/
├── bank-heist/
│   └── access-codes.txt
│   └── blueprints.pdf
│   └── escape-plan.txt
├── heist-project/
│   └── plans/
│       └── strategy.conf
│   └── vault/
│       └── gold.txt
├── devops-file.txt
├── project-config-yaml
└── team-notes.txt
```

## Ownership Changes

| File/Directory | Before | After |
| :--- | :--- | :--- |
| `app-logs/` | root:root  | berlin:heist-team  |
| `bank-heist/access-codes.txt` | root:root  | tokyo:vault-team  |
| `bank-heist/blueprints.pdf` | root:root  | berlin:tech-team  |
| `bank-heist/escape-plan.txt` | root:root  | nairobi:vault-team  |
| `heist-project/` | root:root  | professor:planners  |
| `heist-project/plans/` | root:root  | professor:planners  |
| `heist-project/plans/strategy.conf` | root:root  | professor:planners  |
| `heist-project/vault/` | root:root  | professor:planners  |
| `heist-project/vault/gold.txt` | root:root  | professor:planners  |
| `devops-file.txt` | root:root  | berlin:root  |
| `project-config-yaml` | root:root  | professor:heist-team  |
| `team-notes.txt` | root:root  | root:heist-team  |

## Commands Used
- `ls -l`:- Long list of files or directories.
- `ls -l <file/directory>`:- Full details of a specific file or directory.
- `sudo useradd -m <username>`:- Creates a user with its respective home directory.
- `sudo chown <username> <file/directory>`:- Change owner of the file or directory.
- `sudo groupadd <groupname>`:- Creates a new group.
- `sudo chgrp <groupname> <file/directory>`:- Change the group of particular file or directory.
- `sudo chown owner:group <file/directory>`:- Change owner and group of particular file or directory in a single command.
- `mkdir -p <folder>/<sub-folder>`:- Create nested directories.
- `sudo chown -R owner:group <folder>`:- Change owner and group of the nested folder.
- `ls -lR <folder>`:- Shows nested long list of files or directories.

## What I Learned
- Single user can have multiple groups. Multiple groups can have multiple users.
- We can change the owner and group of particular file or directory in a single command.
- We can create nested sub-folders using `-p` (parent) flag.
- We can change owner and group of the nested folders using `-R` (recurring) flag.
- We can display long list of nested folders using `-lR` (long recurring) flag.
