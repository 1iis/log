# log
Run a command every `n` seconds. See the output in terminal and/or write to disk.

`log` is a barebone alternative to `watch`, better suited for longitudinal monitoring, logging, etc.

## Usage

One file: [`log`](/log), a Bash shell script.

```sh
log [-v] [-n SECONDS] [-o FILE] [-t] [-h] <command>
```
Exit with <kbd>Ctrl</kbd> + <kbd>C</kbd>.

`-v` is a toggle:
- Without it: `log` writes to `<command>.log`  
- With it: `log` outputs to terminal (no log)

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

```sh
log date  # wait a few seconds
^C        # CTRL+C to 

# after four seconds
cat date.log
Tue Apr 28 14:14:33 UTC 2026
Tue Apr 28 14:14:34 UTC 2026
Tue Apr 28 14:14:35 UTC 2026
Tue Apr 28 14:14:36 UTC 2026
```

```sh
log -v date  # no log to file
Tue Apr 28 14:14:51 UTC 2026
Tue Apr 28 14:14:52 UTC 2026
Tue Apr 28 14:14:53 UTC 2026
Tue Apr 28 14:14:54 UTC 2026
...
```

```sh
log -tv date  # Unix timestamps
[1777386029] Tue Apr 28 14:20:29 UTC 2026
[1777386030] Tue Apr 28 14:20:30 UTC 2026
[1777386031] Tue Apr 28 14:20:31 UTC 2026
[1777386032] Tue Apr 28 14:20:32 UTC 2026
...
```

Write the output of `date` with Unix timestamps to both terminal (`-v`) and to a file of our choice (`-o /my/file.log`).
```sh
log -t -v -o /my/file.log date
[1777386138] Tue Apr 28 14:22:18 UTC 2026
[1777386139] Tue Apr 28 14:22:19 UTC 2026
[1777386140] Tue Apr 28 14:22:20 UTC 2026
[1777386141] Tue Apr 28 14:22:21 UTC 2026
^C

cat /my/file.log
[1777386138] Tue Apr 28 14:22:18 UTC 2026
[1777386139] Tue Apr 28 14:22:19 UTC 2026
[1777386140] Tue Apr 28 14:22:20 UTC 2026
[1777386141] Tue Apr 28 14:22:21 UTC 2026
```
