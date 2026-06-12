# Day 10 – File Permissions & File Operations Challenge

## Files Created

Make a directory to work with files.
```bash
mkdir vishakha
```

Go to `vishakha` directory and create a new file.
```bash
cd vishakha
touch devops.txt
```

Create and write into a file.
```bash
cat > notes.txt
```

Create and write into a shell file using `vim`.
```bash
vim script.sh

--

# Insert content

#!/bin/bash

echo "Hello DevOps"
```

Read a file.
```bash
cat notes.txt
```

Read a shell file in vim read-only mode.
```bash
vim -R script.sh
```

Read first 5 lines of a file.
```bash
head -n 5 /etc/passwd
```

Read last 5 lines of a file.
```bash
tail -n 5 /etc/passwd
```

Read the permissions of the file.
```bash
ls -l devops.txt notes.txt script.sh
```

OUTPUT:
```bash
total 12  
-rw-r--r-- 1 sandbox sandbox    0 Jun 12 10:39 devops.txt  
-rw-r--r-- 1 sandbox sandbox   17 Jun 12 10:39 notes.txt  
-rw-r--r-- 1 sandbox sandbox   33 Jun 12 10:41 script.sh  
```

Format: rwxrwxrwx (owner-group-others)

- `r` = read (4), `w` = write (2), `x` = execute (1)

| File/Directory | Permissions | Remarks |
| :--- | :--- | :--- |
| devops.txt | `-rw-r--r--` | Read and write for owners. Read-only for groups and other users. |
| notes.txt | `-rw-r--r--` | Read and write for owners. Read-only for groups and other users. |
| script.sh | `-rw-r--r--` | Read and write for owners. Read-only for groups and other users. |

## Permission Changes

### Before

```bash
total 12  
-rw-r--r-- 1 sandbox sandbox    0 Jun 12 10:39 devops.txt  
-rw-r--r-- 1 sandbox sandbox   17 Jun 12 10:39 notes.txt  
-rw-r--r-- 1 sandbox sandbox   33 Jun 12 10:41 script.sh  
```

### After

Make `script.sh` executable
```bash
chmod +x script.sh
./script.sh # Execute the shell file.
```

Set `devops.txt` to read-only (remove write for all)
```bash
chmod -w devops.txt
```

Set `notes.txt` to `640` (owner: rw, group: r, others: none)
```bash
chmod 640 notes.txt
```

Create directory `project/` with permissions `755`
```bash
mkdir -m 755 project
```

After changing permissions:
```bash
total 12  
-r--r--r-- 1 sandbox sandbox    0 Jun 12 10:39 devops.txt  
-rw-r----- 1 sandbox sandbox   17 Jun 12 10:39 notes.txt
drwxr-xr-x 2 sandbox sandbox 4096 Jun 12 10.57 project
-rwxr-xr-x 1 sandbox sandbox   33 Jun 12 10:41 script.sh  
```

Which means,

| File/Directory | Permissions | Remarks |
| :--- | :--- | :--- |
| devops.txt | `-r--r--r--` | Read-only for all owners. groups and other users. |
| notes.txt | `-rw-r-----` | Read and write for the owner. Read-only for the groups. None for other users. |
| project/ | `drwxr-xr-x` | Read, write and execute for the owner. Read and execute for the groups and other users. |
| script.sh | `-rwxr-xr-x` | Read, write and execute for the owner. Read and execute for the groups and other users. |

## Commands Used

- `mkdir`:- Create a new directory.
- `touch`:- Creates a new file.
- `cat > filename`:- Creates a new file and adds the content within the terminal.
- `vim`:- Editor to create file and add contents.
- `ls -l`:- Long list of the files and directories with permissions, file size, date modified, etc.
- `cat`:- Shortcut for concatenate. Displays content from a file.
- `vim -R`:- View file using vim read-only mode.
- `head -n`:- Displays lines from a file from the top.
- `tail -n`:- Displays lines from a file from the bottom.
- `chmod +x`:- Gives executable permissions to owner, groups and other users.
- `chmod -w`:- Removes writable permissions for owner, groups and other users.
- `chmod 640`:- Gives read, write permission to the owner; read-only permission to the groups; and no permission for other users.
- `mkdir -m <mode> <directory_name>`:- Give permissions as a `mode` while creating a new directory.

## What I Learned

- Got an error `'readonly' option is set` while inserting into file using vim in read-only mode. Which means, when `vim -R` is used, the vim editor opens in read-only mode.
- We cannot execute a shell script without executable permission. If done, we get `bash: ./script.sh: Permission denied` error message.
- Using `ls -l`, we can get a full list of the files and directories including permissions, size, user, group, etc.
- We can change mode or permissions using `chmod`.
