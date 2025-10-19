# Lesson 1 – Understanding the Linux Filesystem

Linux stores everything under the single root directory `/`.  From that starting point the filesystem branches into a number of well‑defined subdirectories.  Being comfortable navigating these directories is the first step toward feeling at home in a shell.

## Key directories

At the top level of the filesystem you will see a handful of standard directories.  Each serves a purpose.  For example, `/bin` holds essential user commands like `ls` and `rm`, `/etc` contains configuration files, `/home` contains user home directories, `/opt` is for optional software, `/tmp` is temporary space, `/usr` holds most user‑land programs and libraries, and `/var` stores variable data such as logs【374421223889011†L142-L156】.  Other important directories include:

- **`/boot`** – kernel and boot loader files.
- **`/dev`** – special device files representing hardware (e.g. `/dev/sda`).
- **`/lib`** – shared libraries and kernel modules.
- **`/media`** and **`/mnt`** – mount points for removable media and manually mounted filesystems.
- **`/proc`**, **`/run`** and **`/sys`** – virtual filesystems exposing kernel and process information【374421223889011†L142-L204】.
- **`/sbin`** – system administration binaries for the super‑user.
- **`/srv`** – data for services provided by the system, such as web servers.

Historically `/usr` stored user home directories but today it contains shared data, applications and documentation.  On modern systems many distributions link `/bin` into `/usr/bin` and `/sbin` into `/usr/sbin` so there is little difference between them【957326284333969†L298-L320】.

Configuration files live in `/etc`.  Examples include `/etc/passwd`, which lists local user accounts, `/etc/group`, which lists groups, and `/etc/fstab`, which controls how filesystems are mounted【374421223889011†L231-L256】.

## Learning objectives

By the end of this lesson you should be able to:

1. Describe the purpose of the main directories under `/`.
2. Use `ls` and `cd` to explore the filesystem.
3. View configuration files like `/etc/passwd`.

## Exercises in the Docker container

This lesson includes a Docker Compose environment based on the Alpine image.  To begin, from this lesson directory run:

```sh
docker compose up -d
```

This command will start a container named `lesson1` in the background.  You can access an interactive shell with:

```sh
docker compose exec lesson ash
```

Use the following exercises to explore the filesystem:

1. **List the root directory.**  Inside the container run `ls /` to see the top‑level directories.  Try adding the `-l` option to view permissions and the `-h` option to humanize sizes (not all Alpine commands support `-h`).
2. **Navigate around.**  Change to `/usr` with `cd /usr`, list its contents, then explore `bin` and `sbin`.  Note that many commands live in `/usr/bin` rather than `/bin`【957326284333969†L298-L320】.
3. **Find configuration files.**  Run `ls /etc` to see the many config files on the system.  Use `cat /etc/passwd` to view the user database【374421223889011†L231-L256】.  What fields does each entry contain?  View `/etc/group` for group definitions.
4. **Identify mount points.**  Examine `/proc/mounts` to see what filesystems are mounted in the container.  Where is `/tmp` and other directories mounted?  Use `cat /proc/mounts | head -n 5` to view the first few entries.

When you are finished exploring you can stop the container with `docker compose down`.
