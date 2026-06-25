# Day 16 – Shell Scripting Basics

### Task 1: Your First Script
1. Create a file `hello.sh`

```bash
vim hello.sh
```

2. Add the shebang line `#!/bin/bash` at the top

```bash
#!/bin/bash
```

3. Print `Hello, DevOps!` using `echo`

```bash
#!/bin/bash

echo "Hello, DevOps!"
```

4. Make it executable and run it

```bash
chmod +x hello.sh
./hello.sh
```

What happens if you remove the shebang line?
- The shell script picks the system's default shell (often sh/Dash), which may break Bash-specific features.

---

### Task 2: Variables
1. Create `variables.sh` with:
   - A variable for your `NAME`
   ```bash
   #!/bin/bash

   NAME="Vishakha"
   ```
   - A variable for your `ROLE` (e.g., "DevOps Engineer")
   ```bash
   #!/bin/bash
   
   NAME="Vishakha"
   ROLE="DevOps Engineer"
   ```
   - Print: `Hello, I am <NAME> and I am a <ROLE>`
   ```bash
   #!/bin/bash
   
   NAME="Vishakha"
   ROLE="DevOps Engineer"
   echo "Hello, I am $NAME and I am a $ROLE"
   ```
2. Try using single quotes vs double quotes — what's the difference?
   - If single quotes is used in `echo`, the output is `Hello, I am $NAME and I am a $ROLE`.
   - If double quotes is used in `echo`, the output is `Hello, I am Vishakha and I am a DevOps Engineer`.

---

### Task 3: User Input with read
1. Create `greet.sh` that:
   - Asks the user for their name using `read`
   ```bash
   #!/bin/bash

   read -p "Enter your name:" NAME
   ```
   - Asks for their favourite tool
   ```bash
   #!/bin/bash

   read -p "Enter your name:" NAME
   read -p "Enter your tool:" TOOL
   ```
   - Prints: `Hello <name>, your favourite tool is <tool>`
   ```bash
   #!/bin/bash

   read -p "Enter your name:" NAME
   read -p "Enter your tool:" TOOL

   echo "Hello $NAME, your favourite tool is $TOOL"
   ```

---

### Task 4: If-Else Conditions
1. Create `check_number.sh` that:
   - Takes a number using `read`
   ```bash
   #!/bin/bash

   read -p "Enter a number: " NUM
   ```
   - Prints whether it is **positive**, **negative**, or **zero**
   ```bash
   #!/bin/bash

   read -p "Enter a number: " NUM

   if [ $NUM -gt 0 ]; then
         echo "$NUM is a positive number"
   elif [ $NUM -lt 0 ]; then
         echo "$NUM is a negative number"
   else
         echo "$NUM is zero."
   fi
   ```

2. Create `file_check.sh` that:
   - Asks for a filename
   ```bash
   #!/bin/bash

   read -p "Enter a filename: " FNAME
   ```
   - Checks if the file **exists** using `-f`
   ```bash
   #!/bin/bash

   read -p "Enter a filename: " FNAME

   if [ -f $FNAME ]; then
         echo "$FNAME exists."
   ...
   ...
   ```
   - Prints appropriate message
   ```bash
   #!/bin/bash

   read -p "Enter a filename: " FNAME

   if [ -f $FNAME ]; then
         echo "$FNAME exists."
   else
         echo "$FNAME does not exist."
   fi
   ```

---

### Task 5: Combine It All
Create `server_check.sh` that:
1. Stores a service name in a variable (e.g., `nginx`, `sshd`)
```bash
#!/bin/bash

SERVICE_NAME="nginx"
```

2. Asks the user: "Do you want to check the status? (y/n)"
```bash
#!/bin/bash

SERVICE_NAME="nginx"

read -p "Do you want to check the status? (y/n) " ANS
```

3. If `y` — runs `systemctl status <service>` and prints whether it's **active** or **not**
```bash
#!/bin/bash

SERVICE_NAME="nginx"

read -p "Do you want to check the status? (y/n) " ANS

if [ $ANS == 'y' ]; then
        systemctl status $SERVICE_NAME
...
...
```

4. If `n` — prints "Skipped."
```bash
#!/bin/bash

SERVICE_NAME="nginx"

read -p "Do you want to check the status? (y/n) " ANS

if [ $ANS == 'y' ]; then
        systemctl status $SERVICE_NAME
elif [ $ANS == 'n' ]; then
        echo "Skipped..."
fi
```

---

### What I learned

- If `#!/bin/bash` is not added to the top of the shell script, the shell script picks the system's default shell (often sh/Dash), which may break Bash-specific features.
- When defining a variable in a shell script, there shouldn't be any spaces i.e. `NAME="Vishakha"`.
- Double quotes should be used instead of single quotes for echo command to run the variables within the message.
- While using `if` command, we should use `[]` brackets with proper spaces i.e. `if[ condition ]; then` and `if` command should end with `fi`.
- We can automate shell commands inside the shell script by using `chmod +x hello.sh` and running it with `./hello.sh`.

