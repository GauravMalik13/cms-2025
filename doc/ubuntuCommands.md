# Ubuntu Commands

## List of Commands

- [export](#export)

- [source](#source)

- [Symbolic links]
(#ln--s-pathtosourcedir-pathtodestinationdir)

- [Listing of files/dirs](#listing-in-terminal)

### `export`

This is a command that is used to set the environment variable in the terminal.
    
Usage:

```bash
export varName="something"
```

### `source`

This is used to run the commands from a file. This could be replaced by `.`. `.` has the same work.

### Symbolic Links

`ln -s /path/to/source/dir /path/to/destination/dir`

This command is used to created a shortcut(symbolic link) of source dir and save it to destination dir. This essentially helps use to escape from writing long paths of dirs again and again.

## Listing in terminal

 Listing Just dirs in the current dir

```bash
ls -d */
```

### Various Flags for `ls`

* `-a` : List all contents of dir including hidden files

* `-h` : human readable format

* `-l` : Long Listing (detailed view)

* `-t` : Sort by time, newest on top

* `-r` : For reverse Order

* `-R` : Recursive (Lists Subdirs)

* `-S` : Sort by file size, largest first.

For more, check `man ls` or `ls --help`

## Bash Scripts

A bash script is a file containing a sequence of commands that are executed by the bash program line by line.
In order to create a bash script, one needs to create a file with `.sh` extension.

```bash
#!/bin/bash

#commands to run 

```

The first line is called `shebang`. It is necessary as it tells the terminal the address of the bash shell and tells the shell to run it using bash.

### `chmod`

This command is used to change the permissions and the modes of execution of the file.

<b>Basic Syntax</b>

```bash
chmod [ugoa...][+-=][perms...] filename
```

The first square bracket `[ugoa...]` selects the user for which permissions need to be changed.

The users can be selected according to the following table:



| Category | Meaning                     |
| -------- | --------------------------- |
| **u**    | User (owner)                |
| **g**    | Group                       |
| **o**    | Others (everyone else)      |
| **a**    | All (user + group + others) |



The second bracket has following function:
* `+` $\rightarrow$ Add permission
* `-` $\rightarrow$ Remove permission
* `=` $\rightarrow$ Set Exact permission

Next is the permissions:



| Permission | Meaning | Symbol |
| ---------- | ------- | ------ |
| **r**      | Read    | 4      |
| **w**      | Write   | 2      |
| **x**      | Execute | 1      |



Examples:

```bash
chmod u+x script.sh      # Add execute for the user
chmod g-w file.txt       # Remove write for group
chmod o=r file.txt       # Give others read only
chmod a+x my_program     # Make executable for everyone
chmod ug+rw data.txt     # Add read+write for user & group
```

### Tmux (persistent sessions)

It lets you run multiple terminal sessions inside one window, and keep them alive even if you disconnect (e.g. SSH logout).

In Lxplus, you can modify the `~/.bashrc` file with the alias : 

```bash
alias tmux_service="systemctl --user start tmux.service"
```

Run the following command before the creating a new tmux seeeion:

```bash
tmux_service
```

Then use the following commands to do stuff: 



| Action             | Command                         |
| -------------------| --------------------------------|
| List sessions      | `tmux ls`                       |
| Create new session | `tmux new -t mysession`         |
| Kill session       | `tmux kill-session -t mysession`|
| Kill all sessions  | `tmux kill-server`              |
| Rename session     | `Ctrl + b` → `$`                |
