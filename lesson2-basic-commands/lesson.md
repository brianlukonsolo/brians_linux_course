# Lesson 2 – Basic Shell Commands

The shell comes with a set of basic commands for navigating the filesystem and manipulating files.  Learning these commands will make you productive quickly.  Many of them accept options; you can always run a command with `--help` to see a summary of its usage【644223875718832†L395-L399】.

## Common commands

- **`pwd`** – Print the current working directory.  Running `pwd` simply writes the full path of the directory you are in【644223875718832†L402-L406】.
- **`cd`** – Change directory.  Use an absolute path (starting with `/`) or a relative path.  Special notations include `.` for the current directory, `..` for the parent, and `~` for your home directory【644223875718832†L428-L440】.  For example, `cd /tmp` jumps to the temporary directory, while `cd ..` moves up one level【644223875718832†L454-L476】.
- **`mkdir`** – Make a new directory.  `mkdir newdir` creates an empty directory【644223875718832†L414-L420】.
- **`touch`** – Create an empty file or update its timestamp.  Running `touch file.txt` will create `file.txt` if it does not exist【644223875718832†L408-L413】.
- **`ls`** – List directory contents.  By default it shows file names.  Use `ls -l` to display permissions, ownership, size and timestamps and `ls -lh` to make sizes human‑readable【644223875718832†L506-L519】.  Many shells support `ls -a` to include hidden files.
- **`cp`** – Copy files or directories.  For example `cp source.txt dest.txt` copies a file.  To copy directories use `cp -r` (recursive) and add `-a` to preserve attributes and permissions【644223875718832†L480-L501】.
- **`mv`** – Move or rename files and directories.  `mv oldname newname` renames a file; `mv file.txt /tmp/` moves it【644223875718832†L520-L527】.
- **`rm`** – Remove files or directories.  `rm file.txt` deletes a file.  Use `rm -r dir` to remove a directory tree.  Be careful – there is no undo【644223875718832†L528-L543】.
- **`cat`** – Concatenate files and print them to the terminal.  Use `cat file.txt` to display a file.  Options `-n` or `-b` number all lines or only non‑blank lines, respectively【644223875718832†L545-L574】.
- **`echo`** – Print its arguments to standard output.  Combined with redirection (see lesson 5) it is useful for creating or appending to files【644223875718832†L545-L574】.

## Learning objectives

1. Use `pwd` and `cd` to navigate the filesystem using both absolute and relative paths.
2. Create, rename, copy and remove files and directories with `mkdir`, `touch`, `cp`, `mv` and `rm`.
3. Read and write text files using `cat` and `echo`.

## Exercises in the Docker container

Start the container for this lesson with:

```sh
docker compose up -d
```

Then open a shell:

```sh
docker compose exec lesson ash
```

Within the container perform the following exercises inside the mounted `/workspace` volume:

1. **Check where you are.**  Run `pwd` to print your current working directory (it should be `/workspace`).
2. **Create a playground directory.**  Run `mkdir playground` to create a new directory.  Use `ls -l` to verify it exists.
3. **Create a file.**  Change into the new directory with `cd playground` and create an empty file named `notes.txt` using `touch notes.txt`【644223875718832†L408-L413】.  Use `ls -l` to view its permissions.
4. **Write some text.**  Append a line of text to `notes.txt` by running `echo "First line" >> notes.txt`.  Append another line.  Display the file with `cat -n notes.txt` to show line numbers【644223875718832†L545-L574】.
5. **Copy and rename files.**  Back in `/workspace` copy `playground/notes.txt` to `playground/notes.bak` using `cp notes.txt notes.bak`.  Then rename `notes.bak` to `archive.txt` using `mv notes.bak archive.txt`【644223875718832†L520-L527】.
6. **Remove files.**  Remove `archive.txt` with `rm archive.txt`【644223875718832†L528-L543】.  Remove the entire `playground` directory with `cd .. && rm -r playground` when you are done practicing.
7. **Use absolute and relative paths.**  Try copying a file using an absolute path (e.g. `cp /workspace/notes.txt /tmp/`) and a relative path (from inside `/workspace`).
8. **Get help.**  Run `ls --help` or `cp --help` to see available options【644223875718832†L395-L399】.  Explore other commands in this lesson using their help pages.

After practising you can shut down the container with `docker compose down`.
