# Phase 2: Text Manipulation & Shell Scripting

This file contains the commands, explanations, warnings, and verification questions we covered during Phase 2.

---

## 2.1 Standard Streams, Pipes, and Redirections

Linux processes automatically open three data streams:
1. **Standard Input (stdin):** Input data sent to the program (Stream ID: `0`, usually keyboard).
2. **Standard Output (stdout):** Normal output of the program (Stream ID: `1`, usually terminal screen).
3. **Standard Error (stderr):** Error messages (Stream ID: `2`, usually terminal screen).

### Operators
* **`>` (Overwrite stdout):** Redirects standard output to a file, overwriting the file if it exists.
  * `echo "Alice" > name.txt`
* **`>>` (Append stdout):** Redirects standard output to a file, appending to the end without deleting old content.
  * `echo "Bob" >> name.txt`
* **`<` (Redirect stdin):** Feeds file content into a command's input stream.
  * `wc -l < name.txt` (Counts lines of raw input without knowing the filename).
* **`2>` (Redirect stderr):** Redirects only error messages to a file.
  * `ls /nonfolder 2> error.log`
* **`2>&1` (Redirect stderr to stdout):** Merges both success and error streams together.
  * `my_command > output.log 2>&1`
  * **Short form:** `my_command &> output.log` (in modern Bash).
* **`|` (Pipe):** Connects the output (`stdout`) of the left command directly into the input (`stdin`) of the right command.
  * `ls /usr/bin | grep "zip" | less`
* **`tee` (Split output):** Prints to screen AND writes to a file simultaneously.
  * `echo "Log" | tee app.log`

---

## 2.2 Text Processing Utilities

### Commands
* **`sort`:** Sorts lines of text.
  * `sort file.txt`: Alphabetical sort.
  * `sort -n file.txt`: Numeric sort.
  * `sort -r file.txt`: Reverse sort.
* **`uniq`:** Filters out duplicate lines.
  * ⚠️ **WARNING:** `uniq` only checks adjacent (neighboring) lines! You must always `sort` the text first before piping to `uniq`.
  * `sort file.txt | uniq`: Removes all duplicates.
  * `sort file.txt | uniq -c`: Counts the number of times each unique line appears.
* **`cut`:** Extracts specific columns/fields.
  * `cut -d ':' -f 1 /etc/passwd` (Extracts 1st column `-f 1` using colon `-d ':'` delimiter).
* **`awk`:** A powerful pattern scanning language. Splits fields by spaces by default.
  * `$1`, `$2`, `$3` represent columns; `$0` represents the whole line.
  * `awk '{print $1}' access.log` (Prints only the first column, e.g., IP addresses).
* **`sed` (Stream Editor):** Finds and replaces text.
  * Substitution syntax: `sed 's/find/replace/g' file.txt`
  * **In-place edit (`-i`):** Modifies the file directly on disk instead of just printing the result to the screen.
    * `sed -i 's/ERROR/FAIL/g' access.log`
  * ⚠️ **DevOps Tip:** If your search query contains paths or URLs with slashes (e.g., `http://`), escape them with `\` (e.g. `http:\/\/\/`), or use a different delimiter like `|` or `#`:
    * `sed -i 's|http://|https://|g' config.txt`

---

## 2.3 Shell Scripting Basics

Shell scripting is writing a sequence of commands in a text file to execute them as an automated program.

### Key Components
1. **The Shebang (`#!/bin/bash`):** Must be the very first line. It tells the kernel to use the Bash interpreter to run this file.
2. **Variables:**
   * Definition: `NAME="John"` (⚠️ **WARNING:** No spaces around the `=` sign!).
   * Usage: `echo "Hello $NAME"` (Prefix with `$`).
3. **Positional Arguments:** Passed to the script upon execution (e.g., `./deploy.sh app dev`):
   * `$0` = Script name (`./deploy.sh`)
   * `$1` = First argument (`app`)
   * `$2` = Second argument (`dev`)
   * `$#` = Total count of arguments passed.
4. **Conditionals (`if` statements):**
   * Syntax:
     ```bash
     if [ "$#" -ne 1 ]; then
       echo "Usage: $0 [dev|prod]"
       exit 1
     fi
     ```
   * ⚠️ **WARNING:** You must leave spaces inside the brackets around the condition! E.g. `[ "$#" -ne 1 ]`.
   * **Comparison Operators:** `-ne` (not equal), `-eq` (equal), `-gt` (greater than), `-lt` (less than).
5. **Exit Codes:** Every command returns a code from `0` to `255` upon completion.
   * `0` = Success (no errors).
   * Non-zero (e.g., `1`, `127`) = Failure (errors occurred).
   * **`$?`**: Special variable containing the exit status of the *last run command*.
   * `exit 1` inside a script forces it to stop and report a failure.

---

## Verification Questions We Covered

1. **Q:** What is the difference between `>` and `>>`?
   * **A:** `>` creates or overwrites the file. `>>` appends text to the end of the file.
2. **Q:** If a command fails and prints an error, will `my_command > output.log` save the error? Why or why not? What should you run?
   * **A:** No, because normal `>` only redirects `stdout` (stream 1) and errors are written to `stderr` (stream 2). To redirect both, run: `my_command > output.log 2>&1` (or `my_command &> output.log`).
3. **Q:** How do you list `/usr/bin`, search for files containing `"zip"`, and view one page at a time?
   * **A:** `ls /usr/bin | grep "zip" | less`
4. **Q:** When defining a variable, why will `USER = "admin"` fail?
   * **A:** Because Bash interprets spaces as commands. The variable name, `=`, and value must have no spaces: `USER="admin"`.
5. **Q:** If you run a script as `./backup.sh database /tmp`, how does it reference `/tmp`?
   * **A:** Using `$2` (the second argument).
6. **Q:** How do you check if the last command failed or succeeded? What code represents success?
   * **A:** Run `echo $?`. An exit code of `0` represents success.
