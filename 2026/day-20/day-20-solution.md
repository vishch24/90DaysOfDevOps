# Day 20 – Bash Scripting Challenge: Log Analyzer and Report Generator

Write a Bash script `log_analyzer.sh` that automates the process of analyzing log files and generating a daily summary report.

### Task 1: Input and Validation
Your script should:
1. Accept the path to a log file as a command-line argument
2. Exit with a clear error message if no argument is provided
3. Exit with a clear error message if the file doesn't exist

```bash

#!/bin/bash

# 1. Accept the path to a log file as a command-line argument
LOG_FILE="$1"

# 2. Exit with a clear error message if no argument is provided
if [ -z "$1" ]; then
    echo "Error: No log file provided."
    echo "Usage: $0 <path_to_log_file>"
    exit 1
fi

# 3. Exit with a clear error message if the file doesn't exist
if [ ! -f "$LOG_FILE" ]; then
    echo "Error: File '$LOG_FILE' does not exist."
    exit 1
fi

LOG_NAME=$(basename "$LOG_FILE")
CURRENT_DATE=$(date +%Y-%m-%d)
REPORT_FILE="log_report_${CURRENT_DATE}.txt"
TOTAL_LINES=$(wc -l < "$LOG_FILE")

echo "Analyzing $LOG_NAME..."
```

Output:
```text
Analyzing sample_log.log...
```

---

### Task 2: Error Count
1. Count the total number of lines containing the keyword `ERROR` or `Failed`
2. Print the total error count to the console

```bash
# 1. Count the total number of lines containing the keyword `ERROR` or `Failed`
ERROR_COUNT=$(grep -iE "ERROR|Failed" "$LOG_FILE" | wc -l)

#2. Print the total error count to the console
echo "Total Error/Failed Count: $ERROR_COUNT"
```

Output:
```text
Total Error/Failed Count: 5
```

---

### Task 3: Critical Events
1. Search for lines containing the keyword `CRITICAL`
2. Print those lines along with their line number

```bash
# 1. Search for lines containing the keyword `CRITICAL`
CRITICAL_EVENTS=$(grep -n "CRITICAL" "$LOG_FILE")

# 2. Print those lines along with their line number
echo "$CRITICAL_EVENTS" | while read -r line; do
    LINE_NUM=$(echo "$line" | cut -d: -f1)
    MESSAGE=$(echo "$line" | cut -d: -f2-)
    echo "Line ${LINE_NUM}: ${MESSAGE}"
done
```

Output:
```text
Line 17: 2026-06-26 11:20:20 [CRITICAL] [API_GATEWAY] [EVT-503] GET /api/v1/checkout - Status 500 Internal Server Error.
```

---

### Task 4: Top Error Messages
1. Extract all lines containing `ERROR`
2. Identify the **top 5 most common** error messages
3. Display them with their occurrence count, sorted in descending order

```bash
# 1. Extract all lines containing `ERROR`
# 2. Identify the **top 5 most common** error messages
TOP_ERRORS=$(grep "ERROR" "$LOG_FILE" | awk '{$1=$2=$3=""; print $0}' | sed 's/^[ \t]*//' | sort | uniq -c | sort -rn | head -5)

# 3. Display them with their occurrence count, sorted in descending order
echo "$TOP_ERRORS"
```

Output:
```text
1 [DATABASE] [EVT-203] Connection pool exhausted. Maximum 100 connections reached.
1 [APP_SERVER] [EVT-302] Internal Server Error: Could not acquire database connection lease.
```

---

### Task 5: Summary Report
Generate a summary report to a text file named `log_report_<date>.txt` (e.g., `log_report_2026-02-11.txt`). The report should include:
1. Date of analysis
2. Log file name
3. Total lines processed
4. Total error count
5. Top 5 error messages with their occurrence count
6. List of critical events with line numbers

```bash
{
    echo "========================================="
    echo "         DAILY LOG ANALYSIS REPORT       "
    echo "========================================="

    # 1. Date of analysis
    echo "Date of Analysis : $CURRENT_DATE"

    # 2. Log file name
    echo "Log File Name    : $LOG_NAME"

    # 3. Total lines processed
    echo "Total Lines      : $TOTAL_LINES"

    # 4. Total error count
    echo "Total Errors     : $ERROR_COUNT"
    echo ""

    # 5. Top 5 error messages with their occurrence count
    echo "--- Top 5 Error Messages ---"
    if [ -z "$TOP_ERRORS" ]; then
        echo "No ERROR messages found."
    else
        echo "$TOP_ERRORS"
    fi
    echo ""

    # 6. List of critical events with line numbers
    echo "--- Critical Events ---"
    if [ -z "$CRITICAL_EVENTS" ]; then
        echo "No CRITICAL events found."
    else
        # Reformat grep -n output (line:text) to match expected challenge format
        echo "$CRITICAL_EVENTS" | while read -r line; do
            LINE_NUM=$(echo "$line" | cut -d: -f1)
            MESSAGE=$(echo "$line" | cut -d: -f2-)
            echo "Line ${LINE_NUM}: ${MESSAGE}"
        done
    fi
    echo "========================================="
} > "$REPORT_FILE"

echo "Report generated successfully: $REPORT_FILE"
```

Output:
```text
Report generated successfully: log_report_2026-06-26.txt

cat log_report_2026-06-26.txt:

=========================================
         DAILY LOG ANALYSIS REPORT
=========================================
Date of Analysis : 2026-06-26
Log File Name    : sample_log.log
Total Lines      : 23
Total Errors     : 5

--- Top 5 Error Messages ---
      1 [DATABASE] [EVT-203] Connection pool exhausted. Maximum 100 connections reached.
      1 [APP_SERVER] [EVT-302] Internal Server Error: Could not acquire database connection lease.

--- Critical Events ---
Line 17: 2026-06-26 11:20:20 [CRITICAL] [API_GATEWAY] [EVT-503] GET /api/v1/checkout - Status 500 Internal Server Error.
```

---

### Task 6 (Optional): Archive Processed Logs
Add a feature to:
1. Create an `archive/` directory if it doesn't exist
2. Move the processed log file into `archive/` after analysis
3. Print a confirmation message

```bash

ARCHIVE_DIR="archive"

# 1. Create an `archive/` directory if it doesn't exist
if [ ! -d "$ARCHIVE_DIR" ]; then
    mkdir "$ARCHIVE_DIR"
fi

# 2. Move the processed log file into `archive/` after analysis
mv "$LOG_FILE" "$ARCHIVE_DIR/"

# 3. Print a confirmation message
echo "Log file '$LOG_NAME' has been archived to '$ARCHIVE_DIR/'."
```

Output:
```text
Log file 'sample_log.log' has been archived to 'archive/'.
```

---

### Complete Code

```bash
#!/bin/bash

# ====Task 1: Input and Validation====

# 1. Accept the path to a log file as a command-line argument
LOG_FILE="$1"

# 2. Exit with a clear error message if no argument is provided
if [ -z "$1" ]; then
    echo "Error: No log file provided."
    echo "Usage: $0 <path_to_log_file>"
    exit 1
fi

# 3. Exit with a clear error message if the file doesn't exist
if [ ! -f "$LOG_FILE" ]; then
    echo "Error: File '$LOG_FILE' does not exist."
    exit 1
fi

LOG_NAME=$(basename "$LOG_FILE")
CURRENT_DATE=$(date +%Y-%m-%d)
REPORT_FILE="log_report_${CURRENT_DATE}.txt"
TOTAL_LINES=$(wc -l < "$LOG_FILE")

echo "Analyzing $LOG_NAME..."

# ====Task 2: Error Count====

# 1. Count the total number of lines containing the keyword `ERROR` or `Failed`
ERROR_COUNT=$(grep -iE "ERROR|Failed" "$LOG_FILE" | wc -l)

#2. Print the total error count to the console
echo "Total Error/Failed Count: $ERROR_COUNT"

# ====Task 3: Critical Events====

# 1. Search for lines containing the keyword `CRITICAL`
CRITICAL_EVENTS=$(grep -n "CRITICAL" "$LOG_FILE")

# 2. Print those lines along with their line number
echo "$CRITICAL_EVENTS" | while read -r line; do
        LINE_NUM=$(echo "$line" | cut -d: -f1)
        MESSAGE=$(echo "$line" | cut -d: -f2-)
        echo "Line ${LINE_NUM}: ${MESSAGE}"
done

# ====Task 4: Top Error Messages====

# 1. Extract all lines containing `ERROR`
# 2. Identify the **top 5 most common** error messages
TOP_ERRORS=$(grep "ERROR" "$LOG_FILE" | awk '{$1=$2=$3=""; print $0}' | sed 's/^[ \t]*//' | sort | uniq -c | sort -rn | head -5)

# 3. Display them with their occurrence count, sorted in descending order
echo "$TOP_ERRORS"

# ====Task 5: Summary Report====

{
    echo "========================================="
    echo "         DAILY LOG ANALYSIS REPORT       "
    echo "========================================="

    # 1. Date of analysis
    echo "Date of Analysis : $CURRENT_DATE"

    # 2. Log file name
    echo "Log File Name    : $LOG_NAME"

    # 3. Total lines processed
    echo "Total Lines      : $TOTAL_LINES"

    # 4. Total error count
    echo "Total Errors     : $ERROR_COUNT"
    echo ""

    # 5. Top 5 error messages with their occurrence count
    echo "--- Top 5 Error Messages ---"
    if [ -z "$TOP_ERRORS" ]; then
        echo "No ERROR messages found."
    else
        echo "$TOP_ERRORS"
    fi
    echo ""

    # 6. List of critical events with line numbers
    echo "--- Critical Events ---"
    if [ -z "$CRITICAL_EVENTS" ]; then
        echo "No CRITICAL events found."
    else
        # Reformat grep -n output (line:text) to match expected challenge format
        echo "$CRITICAL_EVENTS" | while read -r line; do
            LINE_NUM=$(echo "$line" | cut -d: -f1)
            MESSAGE=$(echo "$line" | cut -d: -f2-)
            echo "Line ${LINE_NUM}: ${MESSAGE}"
        done
    fi
    echo "========================================="
} > "$REPORT_FILE"

echo "Report generated successfully: $REPORT_FILE"

# ====Task 6 (Optional): Archive Processed Logs====

ARCHIVE_DIR="archive"

# 1. Create an `archive/` directory if it doesn't exist
if [ ! -d "$ARCHIVE_DIR" ]; then
    mkdir "$ARCHIVE_DIR"
fi

# 2. Move the processed log file into `archive/` after analysis
mv "$LOG_FILE" "$ARCHIVE_DIR/"

# 3. Print a confirmation message
echo "Log file '$LOG_NAME' has been archived to '$ARCHIVE_DIR/'."
```

---

### What I learned
- `grep -iE "ERROR|Failed`: Uses Extended Regular Expressions `-E` to find matches for either term, case-insensitively `-i`.
- `grep -n "CRITICAL`: Locates critical lines and tracks their original placement via the line number flag `-n`.
- `awk '{$1=$2=$3=""; print $0}'`: Clears out the first 3 columns (typically Date, Time, and Log Level fields) so messages can be grouped uniformly regardless of when they occurred.
- `sort | uniq -c | sort -rn`: Sorts the raw text strings, counts unique occurrences `uniq -c`, and re-sorts them numerically in reverse order `sort -rn` to bubbled up the most frequent events.
- Grouping script outputs into a block using `{ ... } > filename` is far cleaner than appending line-by-line with `>>`.
- Validating command line arguments `-z` and file existence `-f` prevents standard errors from failing silently or damaging workflows downstream.
- Using `awk` is essential when you need to accurately count structural message occurrences.
