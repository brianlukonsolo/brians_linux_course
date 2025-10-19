# Lesson 3 – Users and Groups

Linux is a multi‑user operating system.  Every process runs as some user and belongs to at least one group.  Understanding how accounts and groups are organised will help you administer a system and control access to files.

## User types and groups

There are several kinds of user accounts on a typical Linux system.  The **root** account has unrestricted privileges and is used for system administration.  **Regular users** are for day‑to‑day tasks, while **system or service accounts** run background services.  Some distributions also create **guest** or **temporary accounts**【769783712056450†L125-L143】.  Each user belongs to a **primary group** (a default group) and can belong to additional **secondary groups**【769783712056450†L145-L198】.

User information is stored in `/etc/passwd` and group information in `/etc/group`.  Each line in `/etc/passwd` contains a login name, user ID (UID), group ID (GID), home directory and shell【374421223889011†L231-L256】.  You can view these files with `cat`.

## Managing users and groups

On most distributions you can create and manage accounts with commands such as:

- **`useradd username`** – create a new user【769783712056450†L293-L298】.  You can set options such as `-d /home/username` for the home directory or `-u` to choose a UID【769783712056450†L330-L384】.  Alpine’s busybox also provides `adduser` which simplifies creating a user interactively.
- **`passwd username`** – set or change a user’s password【769783712056450†L303-L309】.
- **`usermod`** – modify an existing account.  For example, `usermod -aG groupname username` adds a user to a supplementary group【769783712056450†L330-L384】.
- **`userdel -r username`** – remove a user and its home directory【769783712056450†L386-L394】.
- **`chown user:group file`** – change the ownership of a file or directory【9560499653815†L196-L241】.  The related `chgrp` command changes only the group, and `chmod` (covered in Lesson 4) alters permissions【9560499653815†L249-L266】.

## Learning objectives

1. List the users and groups defined on a system.
2. Add a new user and group, set a password and modify group membership.
3. Change the owner and group of a file.

## Exercises in the Docker container

Start the container for this lesson with:

```sh
docker compose up -d
```

Open a shell:

```sh
docker compose exec lesson ash
```

Perform the following exercises:

1. **Inspect existing accounts.**  Run `cat /etc/passwd | head -n 5` to view the first few user entries【374421223889011†L231-L256】.  Use `cut -d: -f1 /etc/group` to list group names.  Use `id` to see your own UID and group membership.
2. **Create a user.**  Use Alpine’s `adduser` command to add a user named `student`.  For a non‑interactive experience you can run `adduser -D student` which creates the account with defaults.  Alternatively, if `useradd` is available, run `useradd -m student` to create the home directory【769783712056450†L293-L298】.  Set a password with `passwd student`【769783712056450†L303-L309】.
3. **Create a group.**  Create a new group called `training` using `addgroup training` (or `groupadd training`).  Add `student` to this group with `adduser student training` or `usermod -aG training student`【769783712056450†L330-L384】.
4. **Verify membership.**  Run `id student` to verify the primary and supplementary groups of the new user【769783712056450†L145-L198】.  Log in as that user with `su - student` and check the home directory.
5. **Change ownership of a file.**  As root in `/workspace`, create a file `test.txt` using `echo "dummy" > test.txt`.  Use `chown student:training test.txt` to make `student` the owner and group owner【9560499653815†L196-L241】.  Verify the change using `ls -l`.
6. **Clean up.**  Remove the user and its home directory with `userdel -r student` (or `deluser student`).  Remove the group with `groupdel training` if your distribution provides it.

When you are done exploring you can stop the container with `docker compose down`.
