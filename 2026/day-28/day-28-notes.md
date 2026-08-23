
# Revision Day: Everything from Day 1 to Day 27

## Task 1: Self-Assessment Checklist

### Can do confidently

**Linux**
- [x] Navigate the file system, create/move/delete files and directories
- [x] Manage processes — list, kill, background/foreground
- [x] Work with systemd — start, stop, enable, check status of services
- [x] Read and edit text files using vi/vim or nano
- [x] Troubleshoot CPU, memory, and disk issues using top, free, df, du.
- [x] Explain the Linux file system hierarchy (/, /etc, /var, /home, /tmp, etc.)
- [x] Create users and groups, manage passwords
- [x] Set file permissions using chmod (numeric and symbolic)
- [x] Change file ownership with chown and chgrp

**Shell Scripting**
- [x] Write a script with variables, arguments, and user input
- [x] Use if/elif/else and case statements
- [x] Schedule scripts with crontab

**Git & GitHub**
- [x] Initialize a repo, stage, commit, and view history
- [x] Create and switch branches
- [x] Push to and pull from GitHub
- [x] Explain clone vs fork
- [x] Cherry-pick a commit from another branch
- [x] Use GitHub CLI to create repos, PRs, and issues

---

### Need to revisit

**Linux**
- Create and manage LVM volumes
- Check network connectivity — ping, curl, netstat, ss, dig, nslookup
- Explain DNS resolution, IP addressing, subnets, and common ports

**Shell Scripting**

- Write for, while, and until loops
- Define and call functions with arguments and return values
- Use grep, awk, sed, sort, uniq for text processing
- Handle errors with set -e, set -u, set -o pipefail, trap

**Git & GitHub**

- Merge branches — understand fast-forward vs merge commit
- Rebase a branch and explain when to use it vs merge
- Use git stash and git stash pop
- Explain squash merge vs regular merge
- Use git reset (soft, mixed, hard) and git revert
- Explain GitFlow, GitHub Flow, and Trunk-Based Development

---

## Task 2: Revisit Your Weak Spots

**Linux**
1. How to check network connectivity? Explain ping, curl, netstat, ss, dig, nslookup.
   - `ping`:
     - It sends ICMP Echo Requests and waits for ICMP Echo Replies. It is mainly used to check whether a host is reachable and to see round-trip time.
     - It helps answer:
       - Can I reach the machine?
       - How much latency is there?
       - Are packets being lost?
     - A failed `ping` does not automatically mean the server is down. ICMP can be blocked by a firewall while TCP/HTTP access still works.
   - `curl`:
     - It operates at the application/service level. It's extremely useful for testing HTTP/HTTPS APIs and web servers.
     - It helps answer:
       - Can I connect to the web service and get an HTTP response?
   - `netstat`:
     - It displays network connections, routing information, interface statistics and related information.
     - It helps answer:
       - What network connections/ports exist?
     - Example, `netstat -tuln` will give,
       ```text
        Proto Local Address      State
        tcp   0.0.0.0:22         LISTEN
        tcp   0.0.0.0:80         LISTEN
        tcp   0.0.0.0:443        LISTEN
       ```
       This tells something on this machine is listening on ports 22, 80 and 443.
     - what is `-tuln`?
       - `-t` TCP
       - `-u` UDP
       - `-l` listening
       - `-n` numeric output
     - It is older Linux networking tooling. Modern Linux systems generally prefer `ss`, which provides similar information and more TCP/socket details.
   - `ss`:
     - It is used to inspect socket statistics and can show more TCP/state information than traditional tools.
     - It is a modern way to inspect sockets.
     - It helps answer:
       - What services are listening?
       - Who is connected to the system?
       - Which program owns a connection?
       - How much traffic/socket usage is there?
       - What is the state of a connection?
   - `dig`:
     - It is primarily a DNS troubleshooting tool. It'll give DNS information including the answer section.
     - If `ping` fails, you can checking with `dig`.
     - It helps answer:
       - Is DNS resolving correctly?
     - It provides more detailed DNS information and is particularly useful for diagnosing DNS behavior.
     - If DNS doesn't return an address, you have a DNS problem rather than necessarily a network connectivity problem.
   - `nslookup`:
     - It is an another DNS troubleshooting tool which also queries DNS.
     - It is convenient for a quick lookup.
     - It helps answer:
       - What is the Name of a Web Address?
       - Which Server Gives the Data?
       - Is the System Working Right?

2. Explain DNS resolution, IP addressing, subnets, and common ports.
   - **DNS resolution**:
     - It is the process of translating human-readable domain names (like `example.com`) into machine-readable IP addresses (like `192.0.2.1`) so a browser can load a website.
     - This lookup functions like the internet's phonebook, moving through caches and specialized servers to connect users to the correct online server.
   - **IP addressing**:
     - An IP address (Internet Protocol address) is a unique number given to any device like phone or laptop when it connects to a network.
     - It acts just like a digital home address so that data knows where to go.
     - Two types of IP addresses:
       - **Public IP**: The main address a home network uses to talk to the rest of the wide internet.
       - **Private IP**: The hidden local address a router gives to individual devices inside a house (like TV or phone).
   - **subnets**:
     - It is a smaller, separate piece of a larger computer network.
     - If a big network is like a large apartment building, a subnet is like an individual apartment unit with its own room numbers.
     - It keeps things organized, fast, and safe.
     - Uses of subnets:
       - **Better Speed**: Stops data from wandering around everywhere, keeping local traffic inside its own small group.
       - **More Security**: Keeps sensitive areas (like finance computers) locked away from regular guest Wi-Fi.
       - **Easy Organization**: Groups similar devices together, like putting all office printers in one subnet.
   - **common ports**:
     - A network port is like a virtual door on a computer.
     - An IP address finds the correct house on the internet, but a port number tells the mail which specific room or person inside the house should get the package.
     - Examples:
       - **80**: Used for regular, open website pages (HTTP).
       - **443**: Used for safe, encrypted website pages (HTTPS).
       - **53**: Used to look up website names and turn them into numbers (DNS).
       - **22**: Used to log into another computer safely and securely (SSH).

---

**Shell Scripting**
1. What is grep, awk, sed, sort, uniq for text processing?
   - **grep**:
     - It scans through files of text and only stops to show you the exact lines that contain the specific word or pattern you are looking for.
     - Command: `grep "ERROR" log.txt`
   - **awk**:
     - It searches only specific text separated by spaces or commas.
     - Command: `awk '{print $1}' users.txt`
   - **sed** (Stream Editor):
     - It flows through your text and changes words on the fly based on rules you give it, without actually altering the original file unless you tell it to.
     - Command: `sed 's/Dog/Cat/g' animals.txt`
   - **sort**:
     - It takes a jumbled list of text and rearranges the lines so they are perfectly organized.
     - Command: `sort shopping.txt`
   - **uniq**:
     - It looks at adjacent lines of text and removes exact duplicates, leaving you with only unique entries.
     - It only spots duplicates that are right next to each other, so it is almost always paired right after the `sort` command.
     - Command: `sort ips.txt | uniq`

2. How to handle errors with set -e, set -u, set -o pipefail, trap?
   - `set -e`: It tells the script to stop and exit immediately if any command fails.
   - `set -u`: It treats unbound (unassigned) variables as errors and crashes the script.
   - `set -o pipefail`: It ensures that if any command inside a pipe (`|`) fails, the entire pipeline is considered a failure. With `pipefail` combined with `set -e`, the script stops right here.
   - `trap`: It specifies a piece of code to run automatically when the script finishes or encounters an error. Think of it like an airbag deployment or an "Exit" trigger.

---

**Git & GitHub**
1. What are merge branches? Explain fast-forward vs merge commit.
   - Merging branches is the process of combining the work you did on one separate line of code (a branch) back into th main project line.
   - It takes the new changes from a feature branch and joins them together with the main branch.
   - **Fast-Forward Merge**: The main branch has not changed or moved forward since a new separate branch started.
   - **Merge Commit**: The main branch did change and got new updates while working on the separate branch. The two paths have **split** or **diverged**.

2. Explain when to use rebase vs merge.
   - Use **merge** to combine two branches while keeping a complete, safe record of when they met.
   - Use **rebase** to move the local work on top of the latest updates and make the project history a straight, clean line.
   - The golden rule is never to rebase public or shared branches.
  
3. Explain GitFlow, GitHub Flow, and Trunk-Based Development.
   - **GitFlow**:
     - It is a highly structured strategy. It uses multiple "long-lived" branches that serve strict purposes. 
     - The rule is to move code slowly and carefully through an assembly line.
   - **GitHub Flow**:
     - It is a streamlined, lightweight version of GitFlow designed for the cloud era. It ditches the complex develop and release branches entirely.
     - The rule is to keep the **main** branch sacred. Break off, do the work quickly, get a review, and put it right back.
   - **Trunk-Based Development (TBD)**:
     - It is the ultimate strategy for speed and agility. It completely skips long-lived branches and complex code reviews.
     - The rule here is no hoarding code. Merge your work into the main trunk immediately, even if the feature isn't finished yet.

---

## Task 3: Quick-Fire Questions

1. What does `chmod 755 script.sh` do?
   - Changes permissions of **script.sh** to **755**.
   - **755** meaning:
     - **7** for user with all permissions (rwx - Read, Write and Execute).
     - **5** for group with read and execute permissions.
     - **5** for other users with read and execute permissions.

2. What is the difference between a process and a service?
   - A **process** is any program that is currently running on your computer.
   - A **service** is a special, managed type of process that runs quietly in the background, starts automatically, and handles system tasks without needing you to open a window or interact with it.

3. How do you find which process is using port 8080?
   ```bash
   sudo lsof -i :8080
   ```
   This outputs the application name (COMMAND), the PID (Process ID), and the user.

4. What does `set -euo pipefail` do in a shell script?
   - It configures a shell script to fail fast and exit immediately when an error occurs, preventing bugs from cascading silently.

5. What is the difference between `git reset --hard` and `git revert`?
   - `git reset --hard <commit>`:
     - It moves current branch pointer backward to the specified commit.
     - It completely deletes all uncommitted changes, staged files, and any commits made after the target commit.
   - `git revert <commit>`:
     - It creates a brand new commit that introduces the exact opposite changes of the target commit.
     - It leaves your working directory, index, and existing history completely intact.

6. What branching strategy would you recommend for a team of 5 developers shipping weekly?
   - **GitHub Flow (Feature Branching)**: It keeps overhead low, avoids the merge nightmares of complex strategies like GitFlow, and perfectly matches a high-frequency weekly release cadence.

7. What does `git stash` do and when would you use it?
   - It is temporarily shelves (records) uncommitted changes in your working directory so you can work on something else, without committing half-finished code.
   - Use `git stash` when you need to switch contexts, fix an urgent bug, or pull updates but your current code is half-finished and not ready to commit.

8. How do you schedule a script to run every day at 3 AM?
   - Open the Crontab Editor:
     ```bash
     crontab -e
     ```
   - Add the Cron Line:
     ```bash
     0 3 * * * /absolute/path/to/your/script.sh
     ```
   - Save and Exit:
     - **nano** (default for most): Press `Ctrl + O`, `Enter` to save, and `Ctrl + X` to exit.
     - **vim**: Type `:wq` and press `Enter`.

9.  What is the difference between `git fetch` and `git pull`?
    - `git fetch` only downloads those changes to your local database without altering your active workspace.
    - `git pull` automatically merges remote changes into your local files.    

10. What is LVM and why would you use it instead of regular partitions?
    - It is a tool that manages disk space in Linux by creating a flexible virtual pool from physical hard drives.
    - It lets you grow or shrink drives without losing data. It also helps combine many disks into one large volume.

---

## Task 4: Teach It Back

1. How are you going to troubleshoot your Linux system?
   - Assume your Linux system as a restaurant. 
     - The chef is the **CPU** who prepares orders.
     - The kitchen counter space is the **Memory** where ingredients and active orders are kept.
     - The storage room is the **Disk** where the restaurant stores rice, vegetables, containers and other supplies.
   - Suppose, customers complain that orders are delaying. Then, you'll try to troubleshoot the problem with:
     - `top` to check the chef (**CPU**) who is spending most time on preparing a single huge order. This explains CPU's heavy usage.
     - `free` to check that the kitchen counter space (**Memory**) has no space left. This explains memory issue.
     - `df` to check the restaurant's storage room (**Disk**) is almost 95% full. This explains disk space isse.
     - `du` to check which shelves or storage areas are consuming most space and then you remove the used food containers. This explains disk usage issue.

2. Why file permissions are important in Linux?
   - Suppose a company stores its application data in /var/www/application-data.
     - **Developer** needs read and write access.
     - **Project Team** needs read access.
     - **Customers** should have no access.
   - Consider the developer as `owner`, project team as a `group` and customers as `other users`.
   - We use permissions rules like `rwx` which forms **Read(4)**, **Write(2)**, **Execute(1)** to give the permissions.
   - So, if we set `chmod 640 app.py`, 
     - it specifies **6** as read and write permissions for the developer, i.e., `read(4) + write(2)`,
     - **4** as read permission for the project team, i.e., `read(4)`,
     - **0** as no permissions for the customers.
   - File permissions plays an important role in preventing unauthorized access, system stability and integrity, and malware attacks.

3. How the physical internet works?
   - Imagine you click `youtube.com` in the browser. What's happening under the hood?
     - Your browser sends a request to your Wi-Fi router.
     - Your router accepts the request and sends it to your ISP (Internet Service Provider like Jio, Airtel).
     - The request passes through fiber-optic cables.
     - The request is grabbed by the youtube's servers.
     - One of the youtube's servers sends the video data back through the network.
     - Your router collects the data and sends it to your browser.
     - Now, you can watch the video on your browser.
   - And the most surprising part, These fiber-optic cables are handling huge amount of internet traffic deep under the oceans.

4. What is DNS resolution and IP addressing? What's the difference?
   - Imagine your friend's contact number as an IP address.
   - A contact list as DNS where you have saved your friend's contact number as a name.
   - You don't remember your friend's contact number, but you know what name you have saved it.
   - So, you go through your contact list and click on your friend's name.
   - Your friend's name automatically changes to his contact number and dials their number.
   - So,
     - DNS resolution is the mechanism of converting domain names to IP addresses.
     - An IP address is a unique number to identify a device or a network.

5. What is git branching? Why is it important?
   - Consider a tree with various branches.
     - main is the root or the trunk of the tree.
     - feat/login, feat/payment, and bug-fix are the individual branches of the tree which contains application code.
     - When a feature branch works perfectly, we merge the branch back to the main.
     - It helps the project team to work on different feature branches without conflicting with each other.
