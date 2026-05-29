# Day 06 – Linux Fundamentals: Read and Write Text Files

## Creating a file
- Create a directory using `mkdir study-notes` in `/home/ubuntu/`.
- Create a new file `notes.txt` in the same folder using `touch notes.txt` command.

## Writing text to a file
- Write a line of text in `notes.txt` with the command `echo "Line 1" > notes.txt`.
- We can use `tee` command to write and display the written text using `echo "Line 3" | tee -a notes.txt`.

## Appending new lines
- Append a line of text in `notes.txt` with the command `echo "Line 2" >> notes.txt`.

## Reading the file back
- We can use `cat notes.txt` to read the text from the file.
- We can use `head -n 2 notes.txt` to read the first 2 times of the file.
- We can use `tail -n 2 notes.txt` to read the last 2 times of the file.
