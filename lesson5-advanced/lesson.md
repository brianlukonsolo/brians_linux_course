# Lesson 5 – Piping, Processes and Environment Variables

This final lesson introduces additional shell features that are essential for working efficiently on a Linux system.  You will learn how to redirect and pipe command output, monitor and control processes, send signals to terminate programs, and inspect and set environment variables.

## 1. Piping and redirection

Every program started from a shell has three standard streams: **standard input (STDIN)**, **standard output (STDOUT)** and **standard error (STDERR)**【466478427022045†L72-L79】.  You can redirect these streams to files or other programs:

- `command > file` – redirect standard output to a file (overwriting it)【466478427022045†L93-L120】.
- `command >> file` – append standard output to a file【466478427022045†L161-L170】.
- `command < file` – use a file as standard input.
- `command 2> file` – redirect standard error to a file【466478427022045†L328-L344】.
- `command1 | command2` – pipe the standard output of the first command into the standard input of the second.  Multiple pipes allow chaining【466478427022045†L257-L269】.  You can combine pipes and redirection in one command【466478427022045†L275-L279】.

### Exercises

1. In the container create a file containing a list of files in `/bin` by running:

   ```sh
   ls /bin > /workspace/binlist.txt
   ```

   Display the first five lines with `head -n 5 /workspace/binlist.txt`.  Append another listing of `/usr/bin` using `ls /usr/bin >> /workspace/binlist.txt` and verify the file grew【466478427022045†L161-L170】.

2. Use pipes to count how many files start with the letter `p` in `/bin`:

   ```sh
   ls /bin | grep '^p' | wc -l
   ```

   The `grep` command filters lines matching a pattern and `wc -l` counts lines.

3. Find the third entry in `/bin` using a pipeline:

   ```sh
   ls /bin | head -3 | tail -1
   ```

   This chains two pipelines together【466478427022045†L257-L269】.

4. Redirect both standard output and standard error to separate files when running a command that generates errors.  For example:

   ```sh
   ls nonexistent 1>out.txt 2>err.txt
   ```

## 2. Managing processes

Every running program is a **process**.  Use `ps` to view processes owned by your shell.  Running `ps` with no arguments shows processes attached to the current terminal【108447508697910†L28-L44】.  Common options include `-e` (or `-A`) to show all processes, `-f` for a full format and `-o` to customise columns【108447508697910†L50-L62】.  For example, `ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu` shows the most CPU‑hungry processes【108447508697910†L63-L74】.

The **top** command provides a real‑time view of system load and running processes.  It displays system uptime, load averages, CPU and memory usage and a table of processes【108447508697910†L96-L120】.  Useful options include `-b` for batch mode, `-n` to limit iterations, `-p` to monitor specific PIDs and `-o` to sort columns【108447508697910†L121-L142】.

When a process misbehaves you can terminate it with **`kill`**.  Each signal is identified by a number or name; run `kill -l` to list them.  `SIGTERM` (signal 15) requests a graceful shutdown, whereas `SIGKILL` (signal 9) forcibly terminates the process【315745230943713†L164-L181】.

### Exercises

1. Run a long‑lived command in the background with `sleep 60 &`.  Use `ps -e | grep sleep` to find its process ID (PID).  Terminate it gracefully with `kill -15 PID`【315745230943713†L164-L181】.  If it does not exit, force it with `kill -9 PID`.
2. Explore your system’s processes with `ps -ef` and with a customised output, e.g. `ps -eo pid,cmd,%mem --sort=-%mem | head`.  Identify your shell’s PID using `echo $$` and confirm it appears in the process list.
3. Run `top` to view real‑time process information【108447508697910†L96-L120】.  Observe how the CPU and memory percentages change as you start and terminate processes.

## 3. Environment variables

An **environment variable** is a key–value pair that affects the behaviour of the shell and programs.  Unlike shell variables, environment variables have a global scope and are inherited by child processes【451381092467249†L123-L139】.  Use `printenv` or `env` to list them【451381092467249†L153-L170】.  Common variables include `PWD` (current directory), `USER`, `SHELL`, `HOME`, `EDITOR` and `PATH`【451381092467249†L173-L181】.

Print a variable’s value with `echo $VARIABLE` or `printenv VARIABLE`【451381092467249†L186-L206】.  To set a variable temporarily, use `export NAME=value`【451381092467249†L219-L233】.  It will remain defined for the duration of the session and for any child processes.  To persist a variable across sessions, add the `export` line to `~/.profile` or `~/.bashrc` and reload the file using `source ~/.profile`【451381092467249†L254-L299】.

### Exercises

1. Run `printenv | less` to browse the current environment【451381092467249†L153-L170】.  Use `echo $USER` and `echo $HOME` to display individual variables.
2. Set a new environment variable for the current session:

   ```sh
   export MY_VAR="Hello world"
   echo $MY_VAR
   ```

   Confirm that `MY_VAR` is inherited by a child process with `env | grep MY_VAR`.

3. Persist an environment variable.  Edit the file `~/.profile` or `~/.bashrc` (these may not exist in Alpine; you can create them) and add the line `export FAV_COLOR=blue`.  Save the file and run `source ~/.profile`.  Verify that `FAV_COLOR` is available with `echo $FAV_COLOR`【451381092467249†L254-L299】.

## Summary

Pipes and redirection let you build powerful command chains and capture output.  Process management tools such as `ps`, `top` and `kill` allow you to monitor and control programs.  Environment variables customise the behaviour of your shell and are inherited by child processes.  Combining these skills with what you have learned in previous lessons gives you a solid foundation for using and administering a Linux system.
