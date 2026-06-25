# Day 17 – Shell Scripting: Loops, Arguments & Error Handling

### Task 1: For Loop
1. Create `for_loop.sh` that:
   - Loops through a list of 5 fruits and prints each one
   ```bash
   #!/bin/bash

   for fruit in "Apple" "Banana" "Watermelon" "Kiwi" "Mango"; do
         echo $fruit
   done
   ```
2. Create `count.sh` that:
   - Prints numbers 1 to 10 using a for loop
   ```bash
   #!/bin/bash

   for i in {1..10}; do
         echo $i
   done
   ```

---

### Task 2: While Loop
1. Create `countdown.sh` that:
   - Takes a number from the user
   ```bash
   #!/bin/bash

   read -p "Enter a number: " NUM
   ```
   - Counts down to 0 using a while loop
   ```bash
   #!/bin/bash

   read -p "Enter a number: " NUM

   while [ $NUM -ge 0 ]; do
         echo $NUM
         NUM=$(($NUM-1))
   ```
   - Prints "Done!" at the end
   ```bash
   #!/bin/bash

   read -p "Enter a number: " NUM

   while [ $NUM -ge 0 ]; do
         echo $NUM
         NUM=$(($NUM-1))
   done
   echo "Done!"
   ```

---

### Task 3: Command-Line Arguments
1. Create `greet.sh` that:
   - Accepts a name as `$1`
   ```bash
   #!/bin/bash

   if [ -n "$1" ]; then
   ```
   - Prints `Hello, <name>!`
   ```bash
   #!/bin/bash

   if [ -n "$1" ]; then
         echo "Hello, $1!"
   ```
   - If no argument is passed, prints "Usage: ./greet.sh <name>"
   ```bash
   #!/bin/bash

   if [ -n "$1" ]; then
         echo "Hello, $1!"
   else
         echo "Usage: $0 <name>"
   fi
   ```

2. Create `args_demo.sh` that:
   - Prints total number of arguments (`$#`)
   ```bash
   #!/bin/bash

   echo "Total number of arguments: " $#
   ```
   - Prints all arguments (`$@`)
   ```bash
   #!/bin/bash

   echo "Total number of arguments: " $#

   echo "All arguments: " $@
   ```
   - Prints the script name (`$0`)
   ```bash
   #!/bin/bash

   echo "Total number of arguments: " $#

   echo "All arguments: " $@

   echo "Script name: " $0
   ```

---

### Task 4: Install Packages via Script
1. Create `install_packages.sh` that:
   - Defines a list of packages: `nginx`, `curl`, `wget`
   ```bash
   #!/bin/bash

   PACKAGES=("nginx" "curl" "wget")
   ```
   - Loops through the list
   ```bash
   #!/bin/bash

   PACKAGES=("nginx" "curl" "wget")

   echo "Starting package verification..."
   echo "--------------------------------"

   for PKG in "${PACKAGES[@]}"; do
   ...
   ...
   ```
   - Checks if each package is installed (use `dpkg -s` or `rpm -q`)
   ```bash
   #!/bin/bash

   PACKAGES=("nginx" "curl" "wget")

   echo "Starting package verification..."
   echo "--------------------------------"

   for PKG in "${PACKAGES[@]}"; do
      if dpkg -s "$PKG" >/dev/null 2>&1; then
   ...
   ...
   ```
   - Installs it if missing, skips if already present
   ```bash
   #!/bin/bash

   PACKAGES=("nginx" "curl" "wget")

   echo "Starting package verification..."
   echo "--------------------------------"

   for PKG in "${PACKAGES[@]}"; do
      if dpkg -s "$PKG" >/dev/null 2>&1; then
         echo "$PKG is already installed. Skipping..."
   ...
   ...
   ```
   - Prints status for each package
   ```bash
   #!/bin/bash

   PACKAGES=("nginx" "curl" "wget")

   echo "Starting package verification..."
   echo "--------------------------------"

   for PKG in "${PACKAGES[@]}"; do
      if dpkg -s "$PKG" >/dev/null 2>&1; then
         echo "$PKG is already installed. Skipping..."
      else
         echo "$PKG is not installed. Installing now..."

         apt-get install -y "$PKG" >/dev/null 2>&1

         if [ $? -eq 0 ]; then
               echo "Successfully installed $PKG."
         else
               echo "Failed to install $PKG."
         fi
      fi
   done

   echo "--------------------------------"
   echo "All packages processed!"
   ```

---

### Task 5: Error Handling
1. Create `safe_script.sh` that:
   - Uses `set -e` at the top (exit on error)
   ```bash
   #!/bin/bash

   set -e
   ```
   - Tries to create a directory `/tmp/devops-test`
   ```bash
   #!/bin/bash

   set -e

   echo "Starting secure directory setup..."

   mkdir /tmp/devops-test || { echo "Error: Failed to create directory /tmp/devops-test"; exit 1; }
   ```
   - Tries to navigate into it
   ```bash
   #!/bin/bash

   set -e

   echo "Starting secure directory setup..."

   mkdir /tmp/devops-test || { echo "Error: Failed to create directory /tmp/devops-test"; exit 1; }

   cd /tmp/devops-test || { echo "Error: Failed to navigate into /tmp/devops-test"; exit 1; }
   ```
   - Creates a file inside
   ```bash
   #!/bin/bash

   set -e

   echo "Starting secure directory setup..."

   mkdir /tmp/devops-test || { echo "Error: Failed to create directory /tmp/devops-test"; exit 1; }

   cd /tmp/devops-test || { echo "Error: Failed to navigate into /tmp/devops-test"; exit 1; }

   touch test_file.txt || { echo "Error: Failed to create test_file.txt"; exit 1; }
   ```
   - Uses `||` operator to print an error if any step fails
   ```bash
   #!/bin/bash

   set -e

   echo "Starting secure directory setup..."

   mkdir /tmp/devops-test || { echo "Error: Failed to create directory /tmp/devops-test"; exit 1; }

   cd /tmp/devops-test || { echo "Error: Failed to navigate into /tmp/devops-test"; exit 1; }

   touch test_file.txt || { echo "Error: Failed to create test_file.txt"; exit 1; }

   echo "Success: Directory and file created safely!"
   ```

2. Modify your `install_packages.sh` to check if the script is being run as root — exit with a message if not.
   ```bash
   #!/bin/bash

   PACKAGES=("nginx" "curl" "wget")

   if [ "$EUID" -ne 0 ]; then
         echo "Run as root";
         exit 1;
   fi

   echo "Starting package verification..."
   echo "--------------------------------"

   for PKG in "${PACKAGES[@]}"; do
      if dpkg -s "$PKG" >/dev/null 2>&1; then
         echo "$PKG is already installed. Skipping..."
      else
         echo "$PKG is not installed. Installing now..."

         apt-get install -y "$PKG" >/dev/null 2>&1

         if [ $? -eq 0 ]; then
               echo "Successfully installed $PKG."
         else
               echo "Failed to install $PKG."
         fi
      fi
   done

   echo "--------------------------------"
   echo "All packages processed!"
   ```

---

### What I learned

- When we use `{1..n}`, we can range up to `n` numbers.
- We use `$((condition))` to use arithmetic operations.
- `$1` is the very first argument of the script.
- `$#` prints the total number of arguments.
- `$@` prints all arguments.
- `$0` prints script name.
- `dpkg -s` checks if each package is installed.
- `set -e` exits on error.
- `if [ "$EUID" -ne 0 ]; then` checks if the user is root user.
- `if [ $? -eq 0 ]; then` checks whether the very last command executed was successful.
