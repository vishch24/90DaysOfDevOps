# Day 09 Linux User & Group Management Challenge

## Users & Groups Created
- Users: tokyo, berlin, professor, nairobi
  ```bash
  sudo useradd -m tokyo
  sudo useradd -m berlin
  sudo useradd -m professor
  sudo useradd -m nairobi
  ```
- Groups: developers, admins, project-team
  ```bash
  sudo groupadd developers
  sudo groupadd admins
  sudo groupadd project-team
  ```

## Group Assignments
- Add `tokyo` to `developers`
  ```bash
  sudo usermod -aG developers tokyo
  ```
- Add `berlin` to `developers` and `admins`
  ```bash
  sudo usermod -aG developers,admins berlin
  ```
- Add `professor` to `admins`
  ```bash
  sudo usermod -aG admins professor
  ```
- Add `nairobi` and `tokyo` to `project-team`
  ```bash
  sudo gpasswd -M nairobi,tokyo project-team
  ```

## Directories Created

### `/opt/dev-project`

- Set group owner to `developers` and permissions to `775` (rwxrwxr-x)
  ```bash
  sudo chown :developers /opt/dev-project
  chmod 775 /opt/dev-project
  ```
- Test by creating files as `tokyo` and `berlin`
  ```bash
  sudo -u tokyo touch /opt/dev-project/file-by-tokyo.txt
  sudo -u berlin touch /opt/dev-project/file-by-berlin.txt
  ```

### `/opt/team-workspace`

- Set group owner to `project-team`, permissions to `775`
  ```bash
  sudo chown :project-team /opt/team-workspace
  chmod 775 /opt/team-workspace
  ```
- Test by creating file as `nairobi`
  ```bash
  sudo -u nairobi touch /opt/team-workspace/file-by-nairobi.txt
  ```

## Commands Used
- `useradd -m`:- Creates a new user account and their respective home directory.
- `groupadd`:- Creates a new group.
- `usermod -aG`:- Add existing user to the new group without removing them from their current groups.
- `gpasswd -M`:- Add multiple users to a group simultaneously.
- `chown`:- Change owner. In this case, changing owner of a specific directory to a group
- `chmod`:- Change mode. In this case, changing file permission of directories for the groups, i.e., `775`. 
- `sudo -u <username> <command>`:- Execute a specific command as a different user without logging out of the current session.

## What I Learned
- By using `usermod -aG`, we can add the existing user to a specific group without removing them from their groups.
- By using `gpasswd -M`, we can add multiple users to a group simultaneously within a single command.
- By default, groups don't have permissions to write or create files in a specific directory. For that, we need to change permissions for groups, i.e., `775`.
- By using `sudo -u <username> <command>`, we can execute any command as a specific user without logging out of the current sesson.
