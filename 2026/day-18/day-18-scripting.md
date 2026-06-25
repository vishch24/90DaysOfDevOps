# Day 18 – Shell Scripting: Functions & intermediate Concepts

### Task 1: Basic Functions
1. Create `functions.sh` with:
   - A function `greet` that takes a name as argument and prints `Hello, <name>!`
   - A function `add` that takes two numbers and prints their sum
   - Call both functions from the script
   ```bash
   #!/bin/bash

   greet() {
      name="$1"
      echo "Hello, $name!"
   }

   add() {
      num1=$1
      num2=$2
      sum=$((num1 + num2))
      echo "The sum of $num1 and $num2 is: $sum"
   }

   greet "Alice"
   add 5 7
   ```
   Output:
   ```text
   Hello, Alice!
   The sum of 5 and 7 is: 12
   ```

---

### Task 2: Functions with Return Values
1. Create `disk_check.sh` with:
   - A function `check_disk` that checks disk usage of `/` using `df -h`
   - A function `check_memory` that checks free memory using `free -h`
   - A main section that calls both and prints the results
   ```bash
   #!/bin/bash

   check_disk() {
      echo "=== Root Filesystem Usage (/) ==="
      df -h /
   }

   check_memory() {
      echo "=== System Memory Usage ==="
      free -h
   }

   main() {
      echo "=========================================="
      echo "          SYSTEM HEALTH CHECK             "
      echo "=========================================="
      echo ""

      check_disk
      echo ""

      check_memory
      echo ""

      echo "=========================================="
   }

   main
   ```
   Output:
   ```text
   ==========================================
            SYSTEM HEALTH CHECK
   ==========================================

   === Root Filesystem Usage (/) ===
   Filesystem      Size  Used Avail Use% Mounted on
   /dev/sdd       1007G   13G  944G   2% /

   === System Memory Usage ===
                  total        used        free      shared  buff/cache   available
   Mem:           3.7Gi       454Mi       3.0Gi       3.5Mi       258Mi       3.2Gi
   Swap:          1.0Gi          0B       1.0Gi

   ==========================================
   ```

---

### Task 3: Strict Mode — `set -euo pipefail`
1. Create `strict_demo.sh` with `set -euo pipefail` at the top
2. Try using an **undefined variable** — what happens with `set -u`?
3. Try a command that **fails** — what happens with `set -e`?
4. Try a **piped command** where one part fails — what happens with `set -o pipefail`?
```bash
#!/bin/bash
set -euo pipefail

echo "Script started successfully."

echo "My variable is: $UNDEFINED_VAR"

ls /nonexistent_directory_abc123

ls /nonexistent_directory_abc123 | grep "anything"


echo "Script finished successfully."
```
Output:
```text
TEST 1: Undefined Variable (set -u)
Script started successfully.
./strict_demo.sh: line 6: UNDEFINED_VAR: unbound variable

TEST 2: Failing Command (set -e)
Script started successfully.
ls: cannot access '/nonexistent_directory_abc123': No such file or directory

TEST 3: Piped Command Failure (set -o pipefail)
Script started successfully.
ls: cannot access '/nonexistent_directory_abc123': No such file or directory
```

What does each flag do?

- `set -e` → The script will crash immediately because `$UNDEFINED_VAR` does not exist.
- `set -u` → The script will crash immediately because the directory does not exist.
- `set -o pipefail` → Without pipefail, `grep` succeeding would mask the `ls` failure. With pipefail, the failure of `ls` causes the whole script to crash.

---

### Task 4: Local Variables
1. Create `local_demo.sh` with:
   - A function that uses `local` keyword for variables
   - Show that `local` variables don't leak outside the function
   - Compare with a function that uses regular variables
   ```bash
   #!/bin/bash

   fruit_with_local() {
      local fruit="Apple"
      echo "$fruit is inside fruit_with_local()."
   }

   fruit_without_local() {
      other_fruit="Banana"
      echo "$other_fruit is inside fruit_without_local()."
   }

   fruit_with_local

   if [ -z "$fruit" ]; then
         echo "fruit is outside fruit_with_local() and is empty."
   else
         echo "$fruit is outside fruit_with_local()"
   fi

   echo "========================================="

   fruit_without_local

   if [ -z "$other_fruit" ]; then
         echo "other_fruit is outside fruit_without_local and is empty."
   else
         echo "$other_fruit is outside fruit_without_local()"
   fi
   ```
   Output:
   ```text
   Apple is inside fruit_with_local().
   fruit is outside fruit_with_local() and is empty.
   =========================================
   Banana is inside fruit_without_local().
   Banana is outside fruit_without_local()
   ```

---

### Task 5: Build a Script — System Info Reporter
Create `system_info.sh` that uses functions for everything:
1. A function to print **hostname and OS info**
2. A function to print **uptime**
3. A function to print **disk usage** (top 5 by size)
4. A function to print **memory usage**
5. A function to print **top 5 CPU-consuming processes**
6. A `main` function that calls all of the above with section headers
7. Use `set -euo pipefail` at the top
```bash
#!/bin/bash

set -euo pipefail

print_os_info() {
    echo "=== Hostname & OS Info ==="
    echo "Hostname:"
    hostname
    echo "OS Information:"
    cat /etc/os-release | grep "PRETTY_NAME"
}

print_uptime() {
    echo "=== System Uptime ==="
    uptime
}

print_disk_usage() {
    echo "=== Top 5 Filesystems by Size ==="
    df -h | sort -hr | head -n 5
}

print_memory_usage() {
    echo "=== Memory Usage ==="
    free -h
}

print_cpu_processes() {
    echo "=== Top 5 CPU-Consuming Processes ==="
    ps aux --sort=-%cpu | head -n 6
}

main() {
    echo "=========================================="
    echo "            SYSTEM INFO REPORT            "
    echo "=========================================="
    echo ""

    print_os_info
    echo ""

    print_uptime
    echo ""

    print_disk_usage
    echo ""

    print_memory_usage
    echo ""

    print_cpu_processes
    echo ""

    echo "=========================================="
}

main
```
Output:
```text
==========================================
            SYSTEM INFO REPORT
==========================================

=== Hostname & OS Info ===
Hostname:
Vishakha
OS Information:
PRETTY_NAME="Ubuntu 26.04 LTS"

=== System Uptime ===
 16:19:36 up  3:34,  1 user,  load average: 0.00, 0.00, 0.00

=== Top 5 Filesystems by Size ===
tmpfs           374M   12K  374M   1% /run/user/1000
tmpfs           1.9G     0  1.9G   0% /tmp
rootfs          1.9G  2.8M  1.9G   1% /init
none            1.9G  548K  1.9G   1% /run
none            1.9G  4.0K  1.9G   1% /mnt/wsl

=== Memory Usage ===
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       450Mi       3.0Gi       3.5Mi       285Mi       3.2Gi
Swap:          1.0Gi          0B       1.0Gi

=== Top 5 CPU-Consuming Processes ===
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
vishakha    3411  800  0.1   7156  4356 pts/0    R+   16:19   0:00 ps aux --sort=-%cpu
vishakha    3401 18.1  0.0   4944  3644 pts/0    S+   16:19   0:00 /bin/bash ./system_info.sh
root         216  0.1  1.3 1945728 52964 ?       Ssl  12:45   0:24 /usr/bin/containerd
root         329  0.0  2.1 2133344 81848 ?       Ssl  12:45   0:09 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
root         636  0.0  0.0   3188  1248 ?        S    12:45   0:04 /init

==========================================
```

---

### What I learned

- `local` keyword is used inside a function to restrict a variable's visibility to only that specific function.
- `set -e`instructs the shell to exit immediately if any command fails.
- `set -u`instructs the shell to exit immediately if it encounters an unset (uninitialized) variable.
- `set -o pipefail` stops your script from ignoring hidden errors inside a pipeline (commands linked together with `|`).
