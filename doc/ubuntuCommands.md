# Ubuntu Commands

## List of Commands

- [export](#export)
- [source](#source)
- [Symbolic links](#ln--s-pathtosourcedir-pathtodestinationdir)

### `export`

This is a command that is used to set the environment variable in the terminal.
    
Usage:

```bash
export varName="something"
```

### `source`

This is used to run the commands from a file. This could be replaced by `.`. `.` has the same work.

### `ln -s /path/to/source/dir /path/to/destination/dir`

This command is used to created a shortcut(symbolic link) of source dir and save it to destination dir. This essentially helps use to escape from writing long paths of dirs again and again.