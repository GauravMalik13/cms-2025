# Ubuntu Commands

## List of Commands

- [export](#export)
- [source](#source)
- [Symbolic links](#ln--s-pathtosourcedir-pathtodestinationdir)
- [Listing of files/dirs](#listing-in-terminal)
- [Bash Scripts](#bash-scripts)
    - [chmod](#chmod)
- [Tmux](#tmux-persistent-sessions)
- [Basic Commands Summary](#basic-commands)
- [Miscellaneous](#miscelaneous-symbols)


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

<div style="color: red">
Note: While running/writing the file keep in mind that the working directory will be the one which is used to run the script. 
</div>

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


## Basic commands

* `cd`: Change the directory to a different location.
* `ls`: List the contents of the current directory.
* `mkdir`: Create a new directory.
* `touch`: Create a new file.
* `rm`: Remove a file or directory.
* `cp`: Copy a file or directory.
* `mv`: Move or rename a file or directory.
* `echo`: Print text to the terminal.
* `cat`: Concatenate and print the contents of a file.
* `grep`: Search for a pattern in a file.
* `chmod`: Change the permissions of a file or directory.
* `sudo`: Run a command with administrative privileges.
* `df`: Display the amount of disk space available.
* `du -h`: Display the amount of disk space used in the dir. `-h` for human readable format. 
* `history`: Show a list of previously executed commands.
* `ps`: Display information about running processes.


### Miscelaneous symbols

1. Standard streams in bash

    | Stream     | Description     | File Descriptor | Default  |
    | ---------- | --------------- | --------------- | -------- |
    | **stdin**  | Standard input  | `0`             | Keyboard |
    | **stdout** | Standard output | `1`             | Screen   |
    | **stderr** | Standard error  | `2`             | Screen   |

2. Redirecting Output

    | Symbol       | Meaning                               |                                       |
    | ------------ | ------------------------------------- | ------------------------------------- |
    | `>`          | redirect stdout (overwrite)           |                                       |
    | `1>>` or `>>`| redirect **stdout** (append)          |                                       |
    | `2>`         | redirect **stderr** (overwrite)       |                                       |
    | `2>>`        | redirect **stderr** (append)          |                                       |
    | `&>`         | redirect **stdout + stderr** together |                                       |
    | `<`          | take input from file                  |                                       |
    | `/dev/null`  | discard output                        |                                       |

3. Examples

    | Task                        | Command                           |        |
    | --------------------------- | --------------------------------- | ------ |
    | Overwrite file with output  | `ls > files.txt`                  |        |
    | Append output               | `ls >> files.txt`                 |        |
    | Save errors only            | `gcc bad.c 2> compile_errors.log` |        |
    | Save both outputs           | `./run.sh > run.log 2>&1`         |        |
    | Pipe one command to another | `cat file.txt                     | wc -l` |
    | Read input from file        | `wc -l < file.txt`                |        |
