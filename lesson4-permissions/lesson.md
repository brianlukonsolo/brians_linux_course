# Lesson 4 – File and Directory Permissions

Permissions control who can read, write or execute a file or directory.  Understanding and manipulating permissions is essential to maintaining security on a Linux system.

## Interpreting permission strings

When you run `ls -l` the output begins with a string like `-rw-r--r--` or `drwxr-xr-x`.  The first character indicates the type (`-` for a file, `d` for a directory).  The next nine characters are grouped into sets of three representing permissions for the owner, group and others【599141666326684†L109-L123】.  Within each set the letters correspond to **read (r)**, **write (w)** and **execute (x)**.  Directories require execute permission in order to enter them【599141666326684†L91-L107】.

Permissions are often expressed numerically.  Read has the value 4, write has 2 and execute has 1.  Adding these values yields a number from 0 to 7 for each category.  For example, `6` (4 + 2) means read and write, while `7` (4 + 2 + 1) means full permissions【599141666326684†L131-L154】.  A common permission for files is `644` (owner read/write, group and others read).  A typical directory permission is `755` (owner full control, group and others read/execute)【599141666326684†L160-L175】.  Granting `777` permissions (everyone full control) is not recommended.

## Changing permissions and ownership

Use **`chmod`** to change permissions.  You can specify either the numeric mode (e.g. `chmod 600 secrets.txt`) or a symbolic mode.  Symbolic mode uses letters to represent user categories (`u` for user/owner, `g` for group, `o` for others, `a` for all) and operations such as `+` to add, `-` to remove and `=` to set exactly【9560499653815†L249-L266】.  For example:

- `chmod u+rwx,g+rx,o+r myscript.sh` adds execute permission for the owner and group and read permission for others.
- `chmod o-rw file.txt` removes read/write permissions for others.
- `chmod +x script.sh` adds execute permission for everyone.

Use the `-R` option to apply changes recursively to a directory tree.  To change ownership, use **`chown user:group file`** or **`chgrp group file`**【9560499653815†L196-L241】.

## Learning objectives

1. Interpret permission strings in `ls -l` output and understand numeric modes.
2. Use `chmod` to set permissions using numeric and symbolic notation.
3. Understand the implications of directory permissions.
4. Change ownership of files and directories.

## Exercises in the Docker container

Start the container for this lesson with:

```sh
docker compose up -d
```

Then open a shell:

```sh
docker compose exec lesson ash
```

Perform these exercises in the `/workspace` directory:

1. **Inspect default permissions.**  Create a new file with `echo "hello" > example.txt` and list it using `ls -l example.txt`.  Note the permission string.  Use `stat -c "%a" example.txt` (if available) to print the numeric mode.
2. **Modify file permissions.**  Use `chmod 600 example.txt` to restrict the file to the owner only.  Verify the change with `ls -l`.  Then restore typical permissions with `chmod 644 example.txt`【599141666326684†L160-L175】.
3. **Use symbolic modes.**  Make the file executable for the owner by running `chmod u+x example.txt` and check that the execute bit appears【9560499653815†L249-L266】.  Remove execute permission with `chmod u-x example.txt`.
4. **Directory permissions.**  Create a directory `secret` with `mkdir secret` and a file inside it (`echo "top secret" > secret/data.txt`).  Set the directory’s permissions to `700` using `chmod 700 secret`.  Try listing it as another user (use `su nobody` if available) – you should be denied access because the execute bit is required to traverse directories【599141666326684†L91-L107】.
5. **Change ownership.**  If you have a `student` user from Lesson 3, change the owner and group of `example.txt` to `student` and `training` using `chown student:training example.txt`【9560499653815†L196-L241】.  Check the result with `ls -l`.
6. **Cleanup.**  Remove any files or directories you created when finished.

Stop the container when done with `docker compose down`.
