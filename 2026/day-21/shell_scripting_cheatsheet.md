# Day 21 – Shell Scripting Cheat Sheet

### 1. Basics

| Topic | Key Syntax | Example | Explanation |
|-------|-----------|---------|---------|
| Shebang | `#!/bin/bash` | `#!/bin/bash` | Tells the system's default shell to run using bash. If not, it catches the default shell (sh/Dash), which can break Bash-specific features. |
| Permission to execute | `chmod +x <filename>` | `chmod +x script.sh` | Gives execute permissions to run the script. |
| Running a script | `./<filename>`, `bash <filename>` | `./script.sh`, `bash script.sh` | Helps to run the script to execute commands. |
| Comments | `#` | `# This is single-line comment.` | To add comments in the script for detailing. |
| Variables | `$VAR`, `"$VAR"`, `'$VAR'` | `$NAME`, `"$NAME"`, `'$NAME'` | `$NAME`: Prints the value. <br> `"$NAME"`: Prints the string value. <br> `'$NAME'`: Prints the variable name |
| Reading user input | `read` | `read -p "Enter a name" NAME` | Helps to get a input as a variable without declaring it. |
| Command-line arguments | `$0`, `$1`, `$#`, `$@`, `$?` | `$0`, `$1`, `$#`, `$@`, `$?` | `$0`: Script name. <br> `$1`: First argument. <br> `$#`: Total number of arguments. <br> `$@`: All arguments. <br> `$?`: Special argument. |

---

### 2. Operators and Conditionals

| Topic | Key Syntax | Example | Explanation |
|-------|-----------|---------|---------|
| String comparisons | `=`, `!=`, `-z`, `-n` | `"$var1" = "$var2"` <br> `"$var1" != "$var2"` <br> `-z "$var"` <br> `-n "$var` | `=`: Checks if two strings are identical. Evaluates to true if they match. <br> `!=`: Checks if two strings are different. Evaluates to true if they do not match. <br> `-z`: Checks if a string is empty (zero length). Evaluates to true if it is empty. <br> `-n`: Checks if a string is not empty (non-zero length). Evaluates to true if it has content. |
| Integer comparisons | `-eq`, `-ne`, `-lt`, `-gt`, `-le`, `-ge` | `$NUM -eq 0` <br> `$NUM -ne 0` <br> `$NUM -lt 0` <br> `$NUM -gt 0` <br> `$NUM -le 0` <br> `$NUM -ge 0` | `-eq`: Checks if two numbers are identical. Evaluates to true if they match. <br> `-ne`: Checks if two numbers are different. Evaluates to true if they do not match. <br> `-lt`: Checks if the variable is less than a number. Evaluates to true if the variable is less than the number. <br> `-gt`: Checks if the variable is greater than a number. Evaluates to true if the variable is greater than the number. <br> `-le`: Checks if the variable is less than equal to a number. Evaluates to true if the variable is less than equal to the number. <br> `-ge`: Checks if the variable is greater than equal to a number. Evaluates to true if the variable is greater than equal to the number. |
| File test operators | `-f`, `-d`, `-e`, `-r`, `-w`, `-x`, `-s` | `-f "$FILE"` <br> `-d "$FOLDER"` <br> `-e "/path/to/file.txt"` <br> `-r "/path/to/config.cfg"` <br> `-w "/var/log/app.log"` <br> `-x "/usr/local/bin/myscript.sh"` <br> `-s "output.log"` | `-f`: Checks if an item exists AND is a regular file. <br> `-d`: Checks if an item exists AND is a directory. <br> `-e`: Checks if a specific file or directory exists. <br> `-r`: Checks if a file or directory exists AND has read permissions for the user running the script. <br> `-w`: Checks if a file or directory exists AND has write permissions for the current user. <br> `-x`: Checks if a file exists AND has execute permissions (or if a directory can be opened/searched) by the current user. <br> `-s`: Checks if a file exists AND has a size greater than zero bytes. |
| Conditional syntax | `if`, `elif`, `else` | `if [ condition  ]; then` <br> `# Message 1.` <br> `elif [ another_condition  ]; then` <br> `# Message 2.` <br> `else` <br> `# Message 3.` <br> `fi` | `if`: Checks a condition; if true, it runs the code. <br> `elif`: (else if) Checks alternative conditions if previous ones failed. <br> `else`: Runs a fallback block if nothing else matches. <br> `fi`: Closes the entire statement. |
| Logical operators | `&&`, `\|\|`, `!` | `mkdir /tmp/backup && cp photo.jpg /tmp/backup/` <br> `ping -c 1 google.com \|\| echo "Network is down!"` <br> `[[ ! -d $FOLDER ]]` | `&&`: Runs the second command only if the first command completes successfully (exit code 0). <br> `\|\|`: Runs the second command only if the first command fails (exit code non-zero). <br> `!`: Inverts a condition or command status, turning a failure into a success or a success into a failure. |
| Case statements | `case ... esac` | `ANSWER="y"` <br> `case "$ANSWER" in` <br> ` y\|yes)` <br> `echo "Proceeding..."` <br> `;;` <br> `n\|no)` <br> `echo "Stopping..."` <br> `;;` <br> `*)` <br> `echo "Invalid input."` <br> `;;` <br> `esac` | `case` evaluates a value against a list of patterns sequentially, executes the block following the first matching pattern, terminates that block with ;;, and ends the entire structure with `esac`. <br> `)`: Closes the pattern definition string. <br> `\|`: Acts as an OR operator to group multiple patterns together. <br> `;;`: Required at the end of each block to tell the shell to stop and exit the `case` statement. <br> `*)`: The default fallback wildcard case, which catches anything that did not match the previous choices (like `else` in an `if` block). |

---

### 3. Loops

| Topic | Key Syntax | Example | Explanation |
|-------|-----------|---------|-------------|
| For loop — list-based | `for var in list; do ... done` | `for fruit in apple banana cherry; do` <br> `echo "Fruit: $fruit"` <br> `done` | Iterates over a space-separated list of values. `$fruit` holds the current item on each iteration. |
| For loop — C-style | `for (( init; condition; step )); do ... done` | `for (( i=1; i<=5; i++ )); do` <br> `echo "Count: $i"` <br> `done` | Uses C-style syntax with an initializer, condition check, and increment step. Useful when you need an index counter. |
| While loop | `while [ condition ]; do ... done` | `COUNT=1` <br> `while [ $COUNT -le 5 ]; do` <br> `echo "Line $COUNT"` <br> `(( COUNT++ ))` <br> `done` | Runs as long as the condition is true. Requires manual update of the loop variable to avoid infinite loops. |
| Until loop | `until [ condition ]; do ... done` | `COUNT=1` <br> `until [ $COUNT -gt 5 ]; do` <br> `echo "Line $COUNT"` <br> `(( COUNT++ ))` <br> `done` | Opposite of `while` — runs until the condition becomes true. Reads naturally as "keep going until X happens." |
| Loop control — break | `break` | `for i in 1 2 3 4 5; do` <br> `[ $i -eq 3 ] && break` <br> `echo $i` <br> `done` | Immediately exits the entire loop when triggered. The example stops at 2 and never prints 3, 4, or 5. |
| Loop control — continue | `continue` | `for i in 1 2 3 4 5; do` <br> `[ $i -eq 3 ] && continue` <br> `echo $i` <br> `done` | Skips the rest of the current iteration and jumps to the next one. The example prints all numbers except 3. |
| Looping over files | `for file in *.ext; do ... done` | `for file in *.log; do` <br> `echo "Processing: $file"` <br> `done` | Uses glob expansion to iterate over files matching a pattern. If no files match, `$file` will literally be `*.log` — guard with `[ -f "$file" ]` to be safe. |
| Looping over command output | `while read line; do ... done` | `cat /etc/passwd \| while read line; do` <br> `echo "Entry: $line"` <br> `done` | Reads output line by line. The `read` command splits each line into `$line`. Preferred over `for` for command output since `for` splits on spaces, not newlines. |

---

### 4. Functions

| Topic | Key Syntax | Example | Explanation |
|-------|-----------|---------|-------------|
| Defining a function | `function_name() { ... }` | `greet() {` <br> `echo "Hello, World!"` <br> `}` | Defines a reusable block of code. The function body is wrapped in `{ }`. Define before calling — Bash reads top to bottom. |
| Calling a function | `function_name` | `greet` | Invoke by writing the function name without parentheses. No `()` when calling — that's a syntax error. |
| Passing arguments | `$1`, `$2`, `$@` inside the function | `greet_user() {` <br> `echo "Hello, $1!"` <br> `}` <br> `greet_user "Vishakha"` | Arguments are positional: `$1` is the first, `$2` the second. `$@` holds all arguments as separate words. These are local to the function and do not affect the script-level `$1`, `$2`. |
| Return values — `return` | `return N` | `is_even() {` <br> `(( $1 % 2 == 0 )) && return 0 \|\| return 1` <br> `}` <br> `is_even 4 && echo "Even"` | `return` sets the exit code (0–255) only — not a value. Use it for pass/fail signals, not data. Check with `$?` or `&&`/`\|\|`. |
| Return values — `echo` | `echo value` + `$(function_name)` | `get_date() {` <br> `echo "$(date +%F)"` <br> `}` <br> `TODAY=$(get_date)` <br> `echo "Date is $TODAY"` | Use `echo` to return actual data from a function. Capture it with `$()`. This is the standard pattern for returning strings or computed values. |
| Local variables | `local var=value` | `counter() {` <br> `local COUNT=0` <br> `COUNT=$(( COUNT + 1 ))` <br> `echo $COUNT` <br> `}` | Without `local`, variables inside functions are global and can overwrite script-level variables with the same name. Always use `local` for variables that should stay inside the function. |

---

### 5. Text Processing Commands

| Topic | Key Syntax | Example | Explanation |
|-------|-----------|---------|-------------|
| `grep` — basic search | `grep "pattern" file` | `grep "error" app.log` | Prints every line in the file that contains the pattern. Case-sensitive by default. |
| `grep -i` | `grep -i "pattern" file` | `grep -i "error" app.log` | Case-insensitive match. `ERROR`, `Error`, and `error` all match. |
| `grep -r` | `grep -r "pattern" dir/` | `grep -r "TODO" ./src/` | Recursively searches all files inside a directory tree. |
| `grep -c` | `grep -c "pattern" file` | `grep -c "404" access.log` | Prints a count of matching lines, not the lines themselves. |
| `grep -n` | `grep -n "pattern" file` | `grep -n "fail" deploy.log` | Prefixes each matching line with its line number. |
| `grep -v` | `grep -v "pattern" file` | `grep -v "DEBUG" app.log` | Inverts the match — prints lines that do NOT contain the pattern. |
| `grep -E` | `grep -E "pattern1\|pattern2" file` | `grep -E "error\|warn" app.log` | Enables extended regular expressions. Allows `\|` for OR, `+`, `?`, `{n}` quantifiers. |
| `awk` — print columns | `awk '{print $N}' file` | `awk '{print $1, $3}' access.log` | Splits each line into fields (default separator: whitespace). `$1` is the first field, `$NF` is the last. |
| `awk` — field separator | `awk -F 'delim' '{print $N}' file` | `awk -F ':' '{print $1}' /etc/passwd` | `-F` sets a custom field separator. Here `:` splits the passwd file into username, password, UID, etc. |
| `awk` — pattern match | `awk '/pattern/ {action}' file` | `awk '/ERROR/ {print $0}' app.log` | Runs the action block only on lines that match the pattern. `$0` is the entire current line. |
| `awk` — BEGIN/END | `awk 'BEGIN{...} END{...}' file` | `awk 'BEGIN{count=0} /ERROR/{count++} END{print count}' app.log` | `BEGIN` runs once before reading any input. `END` runs once after all input is processed. Useful for initializing and summarizing. |
| `sed` — substitution | `sed 's/old/new/' file` | `sed 's/foo/bar/' file.txt` | Replaces the first occurrence of `old` with `new` on each line. Add `g` flag for global replace: `s/old/new/g`. |
| `sed` — in-place edit | `sed -i 's/old/new/g' file` | `sed -i 's/localhost/127.0.0.1/g' config.cfg` | Edits the file directly without printing to stdout. On macOS, use `sed -i ''` instead. |
| `sed` — delete lines | `sed 'Nd' file` or `sed '/pattern/d' file` | `sed '5d' file.txt` <br> `sed '/^#/d' script.sh` | `Nd` deletes line N. `/pattern/d` deletes all lines matching the pattern. The second example removes comment lines. |
| `cut` — by delimiter | `cut -d 'delim' -f N file` | `cut -d ',' -f 2 data.csv` | Extracts field N from each line using the specified delimiter. Fields are 1-indexed. |
| `cut` — by characters | `cut -c N-M file` | `cut -c 1-10 file.txt` | Extracts characters by position range. `-c 1-10` gives the first 10 characters of each line. |
| `sort` — alphabetical | `sort file` | `sort names.txt` | Sorts lines alphabetically in ascending order (A–Z). |
| `sort` — numerical | `sort -n file` | `sort -n scores.txt` | Sorts numerically. Without `-n`, `10` sorts before `9` (lexicographic order). |
| `sort` — reverse | `sort -r file` | `sort -rn scores.txt` | `-r` reverses the sort order. Combine with `-n` for reverse-numerical. |
| `sort` — unique | `sort -u file` | `sort -u ips.txt` | Sorts and removes duplicate lines in one step. Equivalent to `sort \| uniq`. |
| `uniq` — deduplicate | `uniq file` | `sort ips.txt \| uniq` | Removes consecutive duplicate lines. Must be used after `sort` since it only removes adjacent duplicates. |
| `uniq -c` | `uniq -c file` | `sort access.log \| uniq -c` | Prefixes each line with the count of how many times it appeared. |
| `tr` — translate | `tr 'set1' 'set2'` | `echo "hello" \| tr 'a-z' 'A-Z'` | Replaces each character in set1 with the corresponding character in set2. Reads from stdin only — not a file directly. |
| `tr -d` | `tr -d 'chars'` | `echo "h-e-l-l-o" \| tr -d '-'` | Deletes all occurrences of the specified characters. |
| `wc` — line count | `wc -l file` | `wc -l access.log` | Counts the number of lines. `-w` counts words, `-c` counts bytes, `-m` counts characters. |
| `head` | `head -n N file` | `head -n 20 app.log` | Prints the first N lines. Default is 10 if `-n` is omitted. |
| `tail` | `tail -n N file` | `tail -n 50 error.log` | Prints the last N lines. Default is 10. |
| `tail -f` | `tail -f file` | `tail -f /var/log/syslog` | Follow mode — keeps the file open and prints new lines as they are appended. Useful for live log monitoring. Ctrl+C to stop. |

---

### 6. Useful Patterns and One-Liners

| Topic | Key Syntax | Example | Explanation |
|-------|-----------|---------|-------------|
| Delete files older than N days | `find dir -mtime +N -delete` | `find /var/log -name "*.log" -mtime +30 -delete` | `-mtime +30` matches files last modified more than 30 days ago. `-delete` removes them. Always test without `-delete` first to preview what will be removed. |
| Count lines in all `.log` files | `wc -l *.log` | `wc -l /var/log/*.log \| tail -1` | `wc -l` on multiple files prints per-file counts plus a total. Piping to `tail -1` extracts just the total line. |
| Replace a string across multiple files | `sed -i 's/old/new/g' files` | `find . -name "*.conf" -exec sed -i 's/oldhost/newhost/g' {} +` | Uses `find` to locate all matching files and pipes them into `sed -i` for in-place replacement. The `+` batches files for efficiency. |
| Check if a service is running | `systemctl is-active --quiet service` | `systemctl is-active --quiet nginx \|\| echo "nginx is DOWN"` | `--quiet` suppresses output and sets exit code: 0 if active, non-zero otherwise. The `\|\|` triggers the alert only when the service is not running. |
| Monitor disk usage with alert | `df -h \| awk + threshold check` | `df -h / \| awk 'NR==2 {gsub(/%/,"",$5); if($5>80) print "ALERT: Disk at "$5"%"}'` | `NR==2` skips the header row. `gsub` strips the `%` sign so the value can be compared numerically. Adjust `80` to your threshold. |
| Parse CSV from command line | `awk -F ',' '{print $N}' file.csv` | `awk -F ',' 'NR>1 {print $2, $4}' report.csv` | `NR>1` skips the header row. Extracts columns 2 and 4 from a comma-separated file. For complex or quoted CSVs, use `python3 -c "import csv..."` instead. |
| Tail a log and filter for errors | `tail -f file \| grep --line-buffered "pattern"` | `tail -f /var/log/app.log \| grep --line-buffered -i "error\|warn"` | `--line-buffered` forces grep to flush output immediately instead of batching it, so you see matches in real time. Without it, output may appear delayed. |
| Find the top 10 largest files | `du -ah dir \| sort -rh \| head -10` | `du -ah /var/log \| sort -rh \| head -10` | `du -ah` lists all files with human-readable sizes. `sort -rh` sorts by size descending (human-readable aware). Useful for tracking down what is eating disk space. |

---

### 7. Error Handling and Debugging

| Topic | Key Syntax | Example | Explanation |
|-------|-----------|---------|-------------|
| Exit codes — `$?` | `$?` | `cp source.txt dest.txt` <br> `echo "Exit code: $?"` | Holds the exit code of the last executed command. `0` means success; any non-zero value means failure. Check it immediately after the command — the next command overwrites it. |
| Exit codes — `exit` | `exit N` | `if [ ! -f config.cfg ]; then` <br> `echo "Config missing"` <br> `exit 1` <br> `fi` | Terminates the script with the given exit code. `exit 0` signals success to the caller; `exit 1` (or any non-zero) signals failure. |
| `set -e` | `set -e` | `#!/bin/bash` <br> `set -e` <br> `cp file.txt /backup/` <br> `echo "Done"` | Exits the script immediately if any command returns a non-zero exit code. Without this, the script keeps running after errors silently. Place at the top of the script. |
| `set -u` | `set -u` | `#!/bin/bash` <br> `set -u` <br> `echo $UNDEFINED_VAR` | Treats references to unset variables as an error and exits. Prevents bugs where a typo in a variable name silently expands to an empty string. |
| `set -o pipefail` | `set -o pipefail` | `#!/bin/bash` <br> `set -o pipefail` <br> `cat missing_file.txt \| grep "error"` | By default, a pipeline's exit code is the last command's code — so `grep` succeeding masks `cat` failing. `pipefail` makes the pipeline fail if any command in it fails. |
| Combined safe header | `set -euo pipefail` | `#!/bin/bash` <br> `set -euo pipefail` | The standard "strict mode" header. Combines exit-on-error, unset-variable protection, and pipe failure detection. Use this at the top of every production script. |
| `set -x` — debug mode | `set -x` / `set +x` | `set -x` <br> `cp file.txt /tmp/` <br> `set +x` | Prints each command and its expanded arguments to stderr before executing it. `set +x` turns tracing off. Wrap just the suspected section to reduce noise. |
| `trap` — on exit | `trap 'command' EXIT` | `TMPFILE=$(mktemp)` <br> `trap 'rm -f "$TMPFILE"' EXIT` <br> `echo "data" > "$TMPFILE"` | Registers a command to run automatically when the script exits — for any reason including errors or Ctrl+C. Essential for cleaning up temp files, locks, or open connections. |
| `trap` — on error | `trap 'command' ERR` | `trap 'echo "Error on line $LINENO"' ERR` | Triggered whenever a command exits with a non-zero status. `$LINENO` gives the line number where the error occurred. Combine with `set -e` for comprehensive error reporting. |
| `trap` — on interrupt | `trap 'command' INT` | `trap 'echo "Interrupted! Cleaning up..."; exit 1' INT` | Triggered when the user presses Ctrl+C (SIGINT). Use to perform cleanup before exiting gracefully instead of leaving things in a broken state. |
