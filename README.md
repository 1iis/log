# log
Run a command every `n` seconds. See cumulative outputs (one run by line) in terminal and/or write to disk.

`log` is a barebone alternative to `watch`, better suited for longitudinal monitoring, logging, etc.

## 0. Install
`log` is one file with 20 useful lines of Bash logic.

1. Either clone the repo 1iis/log,  
or download the log file (remove the .txt extension GH adds),  
or just copy it from 3. Source below!  

2. Put the file in your PATH.

```bash
# Pick one:

sudo cp -v log /usr/local/bin  # Copy file

sudo nano /usr/local/bin/log   # Create file, then:
# - paste code (Shift+Ctrl+V) from 2. Source
# - save (Ctrl+O, Enter)
# - exit (Ctrl+X)
```

Make it executable.
```bash
chmod +x /usr/local/bin/log
```

> [!Warning]
> If you use **[Zsh](https://zsh.sourceforge.io/)**, `log` is a built-in function to compute natural $ln$.  
> If if you don't need to do such math in your shell, you can disable it, thereby letting `/usr/local/bin/log` be found by `which log`, and run as expected.
>
> ```sh
> disable log       # ONLY for Zsh
> ...
> ```

## 1. Usage

One file: [`log`](/log), a Bash script.

### Syntax

```sh
log [-v] [-n SECONDS] [-o FILE] [-t] [-h] <command>
```
Exit with <kbd>Ctrl</kbd> + <kbd>C</kbd>.

> [!Important]
> `-v` is a toggle:
> - Without it: `log` **writes** output to `<command>.log` (you see nothing)
> - With it: `log` **prints** output to terminal (no file written)

Use `-v -o FILE` to *both* see outputs in the terminal *and* write to `FILE`.

### Options

| Flag | Description
| --- | ---
| `-v`           | Print output to terminal.<br>If absent, writes to disk.
| `-n SECONDS`   | Interval in `SECONDS` (default: 1).
| `-o FILE`      | Log output to `FILE` (only if `-o` used).
| `-t`           | Prepend each line with Unix timestamp.
| `-h`, `--help` | Show help message and exit.

### Examples

Basic usage: `log` writes to `<command>.log`.
```sh
log date  # wait a few seconds
^C        # CTRL+C to interrupt the process

# after four seconds
cat date.log
Tue Apr 28 14:14:33 UTC 2026
Tue Apr 28 14:14:34 UTC 2026
Tue Apr 28 14:14:35 UTC 2026
Tue Apr 28 14:14:36 UTC 2026
```

`-v` prints to terminal, no write to file.
```sh
log -v date
Tue Apr 28 14:14:51 UTC 2026
Tue Apr 28 14:14:52 UTC 2026
...
```

`-t` for timestamps.
```sh
log -tv date  # Unix timestamps
[1777386029] Tue Apr 28 14:20:29 UTC 2026
[1777386030] Tue Apr 28 14:20:30 UTC 2026
...
```

Write the output of `date` with Unix timestamps to both terminal (`-v`) and to a file of our choice (`-o /my/file.log`).
```sh
log -t -v -o /my/file.log date
[1777386138] Tue Apr 28 14:22:18 UTC 2026
[1777386139] Tue Apr 28 14:22:19 UTC 2026
^C

cat /my/file.log
[1777386138] Tue Apr 28 14:22:18 UTC 2026
[1777386139] Tue Apr 28 14:22:19 UTC 2026
```

`log` in the background to retrieve usage of the terminal session.
```sh
log date &  # pipe with `&` to run process in the background

# do what you want
whoami
pwd
# ...

fg              # retrieve control of the background process
^C              # Ctrl+C to interrupt the process
cat date.log    # check that logging worked
Thu Apr 30 08:38:32 CEST 2026
Thu Apr 30 08:38:33 CEST 2026
Thu Apr 30 08:38:34 CEST 2026
Thu Apr 30 08:38:35 CEST 2026
```

