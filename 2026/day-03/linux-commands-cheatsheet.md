# Day 03 – Linux Commands Cheatsheet

- `mkdir vishakha`: Creates a new directory.
- `mkdir -p demo`: Checks if the folder already exists or not. If not, it creates a new directory.
- `df -h`: Displays disk-space in human-readable format.
- `free -h`: Displays total amount of free and RAM in human-readable format.- 
- `touch hello.txt`: Creates a new file.
- `cat hello.txt`: Displays content from the file. 
- `echo "Hello World" > hello.txt`: Adds the text into the file overwritting the file contents.
- `echo "How are you?" >> hello.txt`: Appends the text into the file.
- `vim hello.txt`: Opens a text editor within the CLI to edit the file.
  - Type `i` to insert.
  - Click `ESC` button to get out fo the edit mode.
  - Then `:wq` to save the file and quit. (To quit the editor without saving any changes, use `:q!` in vim editor)
- `head hello.txt -n 3`:  Displays first 3 lines of the file. (`head` displays first 'n' number of lines irrespective of number mentioned.)
- `tail hello.txt -n 3`:  Displays last 3 lines of the file. (`tail` displays last 'n' number of lines irrespective of number mentioned.)
