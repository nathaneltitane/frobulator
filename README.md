![frobulator](https://raw.githubusercontent.com/nathaneltitane/frobulator/main/frobulator.svg)

[![Donate](https://img.shields.io/badge/Paypal-2f343f.svg?style=for-the-badge&logo=paypal&label=Donate)](https://www.paypal.com/donate?hosted_button_id=ZW3CDCANHJCWJ)

[[ Frobulator // Project Page ]](https://github.com/nathaneltitane/frobulator) [ Version // 2026-10-02 ]

---

### Welcome to [Frobulator](https://frobulator.app)

Frobulator is a custom shell parser and scripting function library: Frobulate all the things!

Frobulator is easy to use and understand and is meant to help streamline your shell scripting projects while providing you with:

- Colorized prompts
- Line header markers for various message types
- Interactive counters and timers
- Interactive progress and process feedback
- Standardized 80 character line parsing
- Character limit overflow handling and line splitting with paragraph formatting
- Standardized user input prompts
- Standardized alphabetical input prompts
- Standardized numerical input prompts
- Streamlined file and directory commands
- POSIX-compliant/compatible
- BASH-centric scripting commands and functions:
   - Customized Debian-based system commands (i.e.: apt/apt-get package commands)
   - Streamlined package management functions that declutter your scripted setups for the most commonly used apt/aptitude commands
   - Dependency functions that simplify package requirements being fetched for all your scripting and project needs
   - Countdown and progress items to add to your scripts
   - Customizable password obfuscation prompts
   - Script checkpoint solutions to iterate over only failed elements or modules
   - Streamlined archive detection and extraction routines
   - Clean logging, redirection and silencing functions for pretty execution and informed debugging

**...all while making redundant and complex code bits a thing of the past!**

### Note:

The current set of assertions upon which Frobulator is built restricts its functionality to scripts exclusively, at least for the time being.

### Usage:

## source

```bash
. "${HOME}"/.local/bin/frobulator
```

## install

Running the script directly installs it to `${HOME}/.local/bin` - no arguments default to `--install`. Sourcing is unaffected: the options only apply when the file is executed.

```bash
./frobulator              # install (default)
./frobulator --install    # install
./frobulator --help       # usage
```

## version

The release is identified by the literal `update="YYYY-MM-DD-HHMM"` line at the top of the file. The bootstrap reads it as text (`grep '^update='`) and downloads a new copy when the online value sorts after the installed one - bump it (four-digit time) for every release, including same-day releases. `version` is derived from it.

## standard script bootstrap

```bash
#!/bin/bash

# dependencies ─────────────────────────────────────────────────────────────────

script="$(basename -- "${BASH_SOURCE[0]}")"

# script arguments for escalation

self_arguments=("${@}")

if [ "${frobulator_loaded}" != "true" ]
then
	echo
	echo "[  >  ] Checking..."
	echo

	# root through sudo keeps using the user's frobulator

	if [ -n "${SUDO_USER}" ]
	then
		USER="${SUDO_USER}"
		HOME="$(getent passwd "${SUDO_USER}" | cut -d ':' -f 6)"
	fi

	frobulator="${HOME}/.local/bin/frobulator"
	frobulator_download="$(mktemp)"

	mkdir -p "${HOME}/.local/bin"

	# latest release - curl or wget - local copy replaced only by a newer one

	curl -s -L -o "${frobulator_download}" get.frbltr.app 2> /dev/null ||
	wget -q -O "${frobulator_download}" get.frbltr.app 2> /dev/null

	version_online="$(grep -m 1 '^update=' "${frobulator_download}")"
	version_local="$(grep -m 1 '^update=' "${frobulator}" 2> /dev/null)"

	if [[ "${version_online}" > "${version_local}" ]]
	then
		mv -f "${frobulator_download}" "${frobulator}"

		chmod +x "${frobulator}"
	fi

	rm -f "${frobulator_download}"

	if [ ! -f "${frobulator}" ]
	then
		echo "[  !  ] Unable to fetch frobulator ── [ get.frbltr.app ]"
		echo

		exit 1
	fi

	source "${frobulator}"
fi

# superuser ────────────────────────────────────────────────────────────────────

frobulator.escalate

# script ───────────────────────────────────────────────────────────────────────

frobulator.script

# version ──────────────────────────────────────────────────────────────────────

version="YYYY-MM-DD"

# usage ────────────────────────────────────────────────────────────────────────

# variables ────────────────────────────────────────────────────────────────────

# defaults ─────────────────────────────────────────────────────────────────────

# functions ────────────────────────────────────────────────────────────────────

# update ───────────────────────────────────────────────────────────────────────

# frobulator.update

# frobulator.upgrade

# requirements ─────────────────────────────────────────────────────────────────

list=(

)

frobulator.require ${list[@]}

list=()

# configuration ────────────────────────────────────────────────────────────────
```
## three-character functions

Short names for the functions used most: prompt formatting and line markers. Each name abbreviates what it does:

| function | stands for |
|---|---|
| `frobulator.plo` | prompt line overwrite |
| `frobulator.pmt` | prompt management tool |
| `frobulator.brk` | line break - carry-over line markers (nul / ind) |
| `frobulator.nul` | null line - carry-over: keep color of the line above |
| `frobulator.ind` | index line - carry-over: without color |
| `frobulator.nil` | empty (nil) line |
| `frobulator.inf` | information |
| `frobulator.wrn` | warning |
| `frobulator.msg` | message |
| `frobulator.add` | add |
| `frobulator.rem` | remove |
| `frobulator.ret` | retain |
| `frobulator.rel` | release |
| `frobulator.fwd` | forward |
| `frobulator.rev` | reverse |
| `frobulator.stp` | stop |
| `frobulator.dwl` | download |
| `frobulator.upl` | upload |
| `frobulator.lnk` | link |
| `frobulator.scs` | success |
| `frobulator.err` | error |
| `frobulator.ins` | insert |
| `frobulator.cpt` | complete |
| `frobulator.url` | url |
| `frobulator.qst` | question prompt |
| `frobulator.ipt` | input prompt |
| `frobulator.usr` | user prompt |
| `frobulator.ltr` | letter |
| `frobulator.num` | number |
| `frobulator.sep` | separator |
| `frobulator.ntf` | notify |

### frobulator.plo

[ p ] rompt [ l ] ine [ o ] verwrite:

Clears a defined number of lines above the current cursor position, wiping from that point down to the end of the screen — built on `terminal_cursor_up` and `terminal_clear_down`.

```bash
frobulator.plo 1
```

### frobulator.pmt

[ p ] rompt [ m ] anagement [ t ] ool:

The shared prompt-formatting engine behind every color and marker command. Normalizes 1–3 arguments (`begin`, `end`, `span character`) into a line as wide as the terminal (`columns_terminal`, never below 80): pads/fills the middle span, folds `begin` with word-detection when it would overflow the line, and truncates an overlong `end` with an ellipsis while preserving its surrounding bracket style - a `[ 'name' // value ]` bracket keeps its whole ` // value ]` tail (and the closing quote), so only the name is shortened. Populates the `prompt_string` array consumed by every wrapper below rather than printing directly.

```bash
frobulator.pmt "Downloading" "[ package.tar.gz ]"
```

### frobulator.brk

Line break: prints the blank line that follows every marker line and notice block, and flags it so the carry-over markers (`frobulator.nul`, `frobulator.ind`) can rejoin the line above instead. Called by the markers themselves - use it directly only after your own `echo` output. It also adds the marker line and its blank line to `lines_printed_terminal` (wrapped rows counted at the real terminal width), which `frobulator.escalate` uses to clear its output.

```bash
frobulator.brk
```


### prompt marker commands

These commands print standard frobulator markers by calling `frobulator.pmt` directly and prefixing its own colored marker glyph (e.g. `[  i  ]`, `[  !  ]`). Most accept a message, an optional detail string, and an optional fill character.

Every marker line is followed by a blank line, printed by `frobulator.brk` - scripts do not add `echo` after messages. Carry-over lines (`frobulator.nul`, `frobulator.ind`) rejoin the line above on screen, so a message and its details stay together. Prompts (`qst`, `ipt`, `usr`) stay on the same line for the answer.

| command | purpose | example |
|--------------------|-----------------------------|---------------------------------------------|
| `frobulator.nil` | empty marker line | `frobulator.nil "Message" "[ detail ]"` |
| `frobulator.inf` | information line | `frobulator.inf "Message" "[ detail ]"` |
| `frobulator.wrn` | warning line | `frobulator.wrn "Message" "[ detail ]"` |
| `frobulator.msg` | message line | `frobulator.msg "Message" "[ detail ]"` |
| `frobulator.add` | add/create line | `frobulator.add "Message" "[ detail ]"` |
| `frobulator.rem` | remove/delete line | `frobulator.rem "Message" "[ detail ]"` |
| `frobulator.ret` | retain/keep line | `frobulator.ret "Message" "[ detail ]"` |
| `frobulator.rel` | release line | `frobulator.rel "Message" "[ detail ]"` |
| `frobulator.fwd` | forward/progress line | `frobulator.fwd "Message" "[ detail ]"` |
| `frobulator.rev` | reverse/back line | `frobulator.rev "Message" "[ detail ]"` |
| `frobulator.stp` | stop line | `frobulator.stp "Message" "[ detail ]"` |
| `frobulator.dwl` | download line | `frobulator.dwl "Message" "[ detail ]"` |
| `frobulator.upl` | upload line | `frobulator.upl "Message" "[ detail ]"` |
| `frobulator.lnk` | link line | `frobulator.lnk "Message" "[ detail ]"` |
| `frobulator.scs` | success line | `frobulator.scs "Message" "[ detail ]"` |
| `frobulator.err` | error line | `frobulator.err "Message" "[ detail ]"` |
| `frobulator.ins` | insert/input line | `frobulator.ins "Message" "[ detail ]"` |
| `frobulator.cpt` | complete line | `frobulator.cpt "Message" "[ detail ]"` |
| `frobulator.url` | url line | `frobulator.url "https://example.com"` |
| `frobulator.qst` | question prompt (no newline) | `frobulator.qst "Enter value"` |
| `frobulator.ipt` | input prompt (no newline) | `frobulator.ipt "Enter value"` |
| `frobulator.usr` | user prompt (no newline) | `frobulator.usr "Enter value"` |
| `frobulator.nul` | continue line, retain color | `frobulator.nul "continued output"` |
| `frobulator.ind` | continue line, clear color | `frobulator.ind "continued output"` |

### argument handling

Most frobulator prompt commands accept either direct string arguments or array-expanded arguments.

Direct arguments:

```bash
frobulator.inf "Checking dependencies" "[ curl ]"
```

Array arguments:

```bash
prompt_arguments=(
	"Checking dependencies"
	"[ curl ]"
)

frobulator.inf "${prompt_arguments[@]}"
```

Output:

```text
[  i  ] Checking dependencies ───────────────────────────────────────── [ curl ]
```

This pattern applies to most commands that forward their arguments through `frobulator.pmt`, including color commands, marker commands, and structured prompt helpers.

Arguments may be provided as:

- Direct strings
- Variables
- Arrays
- Command substitutions that return raw values
- Values generated elsewhere in the script

Examples:

```bash
message="Checking dependencies"
detail="[ curl ]"

frobulator.inf "${message}" "${detail}"
```

```bash
package=$(basename "${archive_path}")

frobulator.inf "Processing package" "[ ${package} ]"
```

```bash
prompt_arguments=(
	"Checking dependencies"
	"[ curl ]"
)

frobulator.fwd "${prompt_arguments[@]}"
```

```bash
current_user=$(id -u -n)

frobulator.wrn "Current user" "[ ${current_user} ]"
```

All argument types are normalized internally through `frobulator.pmt`, allowing prompt formatting, alignment, wrapping, and span generation to remain consistent regardless of how values are supplied.

### frobulator.ltr

Prints a lettered step marker — first argument is the letter, remaining arguments are forwarded to `frobulator.pmt`.

```bash
frobulator.ltr "a" "Select source directory"
```

### frobulator.num

Prints a numbered step marker — first argument is the number, remaining arguments are forwarded to `frobulator.pmt`. An optional palette color before the number prints the number and the line in that color - e.g. green for files to add and red for files to remove, matching their `frobulator.add` / `frobulator.rem` headers.

```bash
frobulator.num "1" "Install dependencies"
frobulator.num lime "1" "new-track.mp3"
frobulator.num crimson "2" "old-track.mp3"
```

### frobulator.sep

Prints a full-width separator line built from `frobulator.pmt`. The line is uncolored unless an optional palette color is given first, which prints the line and its text in that color.

```bash
frobulator.sep
frobulator.sep lime "Section"
```

### frobulator.ntf

Prints a framed notice block. Optional leading arguments select a frame `style` (`square` [default], `round`, `heavy`, `double`, `dots`, `matrix`, `tech`, `skel`, `ascii`), the `split` keyword (renders title and message as two separate frames instead of one divided frame), and a marker-type keyword (`inf`, `wrn`, `scs`, `err`, etc.) to color the title using that marker's color. Remaining arguments are `title` then `message`; a second marker-type keyword placed between them colors the message. Like the line markers, the block is followed by a blank line.

```bash
frobulator.ntf round inf "Notice" "The setup process is ready."
```

```bash
frobulator.ntf heavy split err "Failure" "Could not reach the update server."
```

```bash
frobulator.ntf wrn "Disclaimer" inf "This action is permanent."
```

## prompt formatting

### frobulator.erase

Thin wrapper around `frobulator.plo` — erases a defined number of lines above the current cursor position the same way.

```bash
frobulator.erase 3
```

### frobulator.columns

Sets `columns_terminal` - the width used by prompts, notifications, bars and images - from the terminal width (`tput cols`), never below `columns_terminal_minimum` (default `80`), which is also used when output is not a terminal. Runs on load and again on every window resize (`WINCH` trap), so new output follows the window width.

```bash
columns_terminal_minimum=100
frobulator.columns
```

## color commands

Each named color wrapper calls `frobulator.pmt` directly, stores its color into `prompt_string`, and echoes the result — so every color wrapper shares the same prompt-formatting rules as `frobulator.pmt` above.

```bash
frobulator.[color] "[string]" "[string]" "[span character]"
```

| command | example |
|-----------------------|---------------------------------------------------|
| `frobulator.black` | `frobulator.black "Highlighted" "[ value ]"` |
| `frobulator.silver` | `frobulator.silver "Highlighted" "[ value ]"` |
| `frobulator.grey` | `frobulator.grey "Highlighted" "[ value ]"` |
| `frobulator.white` | `frobulator.white "Highlighted" "[ value ]"` |
| `frobulator.red` | `frobulator.red "Highlighted" "[ value ]"` |
| `frobulator.crimson` | `frobulator.crimson "Highlighted" "[ value ]"` |
| `frobulator.green` | `frobulator.green "Highlighted" "[ value ]"` |
| `frobulator.lime` | `frobulator.lime "Highlighted" "[ value ]"` |
| `frobulator.yellow` | `frobulator.yellow "Highlighted" "[ value ]"` |
| `frobulator.orange` | `frobulator.orange "Highlighted" "[ value ]"` |
| `frobulator.blue` | `frobulator.blue "Highlighted" "[ value ]"` |
| `frobulator.navy` | `frobulator.navy "Highlighted" "[ value ]"` |
| `frobulator.magenta` | `frobulator.magenta "Highlighted" "[ value ]"` |
| `frobulator.purple` | `frobulator.purple "Highlighted" "[ value ]"` |
| `frobulator.fuschia` | `frobulator.fuschia "Highlighted" "[ value ]"` |
| `frobulator.pink` | `frobulator.pink "Highlighted" "[ value ]"` |
| `frobulator.aqua` | `frobulator.aqua "Highlighted" "[ value ]"` |
| `frobulator.teal` | `frobulator.teal "Highlighted" "[ value ]"` |

### frobulator.palette

Adds a named color to the palette at run time from a 256-color code (`0` to `255`). The name is then accepted wherever a palette color name is - the optional color of `frobulator.num` and `frobulator.sep`, and the critter. Like the built-in colors, the sequence is set only when output is a terminal (via `tput`, or ANSI without it) and is empty otherwise. Adding an existing name updates its color.

```bash
frobulator.palette cornflower 68
frobulator.num cornflower "1" "Soft blue line" "[ value ]"
```

## structured prompt helpers

### frobulator.separate

Prints a predefined `frobulator.sep` line (which prints its own blank line after it) — use to separate instructions or warnings from prompts.

```bash
frobulator.separate
```

### frobulator.read

Wraps the `read` builtin (forwarding all arguments to it) and prints a trailing blank line, so prompts stay evenly spaced after user input.

```bash
frobulator.qst "Continue?" "[ y/n ]"
frobulator.read reply
```

### frobulator.script

Prints a script startup banner: a `msg`-colored notice block titled `Script` showing `Executing: '${script}'` and `Version:`. An optional description is shown above them.

```bash
frobulator.script
```

```bash
frobulator.script "Welcome to the Gutendex terminal library!"
```

### frobulator.type

Prints a string one character at a time with a randomized delay between each, to emulate human typing. First argument is the maximum random interval in tenths of a second (defaults to `2`, i.e. up to 0.2 s per character), second argument is the string.

```bash
frobulator.type 3 "Preparing environment..."
```

### frobulator.timeout

Sleeps for a number of seconds (defaults to `1` when omitted) — a simple pause between commands.

```bash
frobulator.timeout 5
```

### frobulator.clear

Pauses for `frobulator.timeout`'s default interval, then clears the terminal — used for script "paging" once a step's checkpoints are met.

```bash
frobulator.clear
```

### frobulator.countdown

Shows a live countdown before continuing, validating that the first argument is a non-negative integer. Remaining arguments are forwarded to `frobulator.pmt` as the message shown beside the counter.

```bash
frobulator.countdown 10 "Starting install" "[ press ctrl+c to cancel ]"
```

## process and progress helpers

### frobulator.action

Generates a randomly colored, grammatically conjugated action prompt from a verb (e.g. `download` → `Downloading...`, `panic` → `Panicking...`), falling back to `frobulate` when no verb is given. Handles common English suffix rules (`-ie` → `-y`, silent `-e` drop, `-ic` → `-ick`, and final consonant doubling for one-syllable verbs ending consonant-vowel-consonant plus a short list of verbs stressed on the last syllable, e.g. `strip` → `Stripping`, `submit` → `Submitting`, but `open` → `Opening`) before appending `-ing`. Sets the `prompt_progress`/`color_progress` globals consumed by `frobulator.progress`. Callable directly, but normally invoked internally.

```bash
frobulator.action "download" "[ ${file} ]"
```

### frobulator.process

Waits on the most recently backgrounded process (`${!}`), animating a simple `/\` ticker beside a message until it exits, then returns that process's exit status.

```bash
apt-get update &
frobulator.process "Updating package index"
```

### frobulator.progress

Same as `frobulator.process`, but generates its message via `frobulator.action` (so the first argument is a bare verb, not a pre-built message) and animates a smoother `⎺⎻⎼⎽⎼⎻` ticker.

```bash
curl -L "${url}" -o "${file}" &
frobulator.progress "download" "[ ${file} ]"
```

### frobulator.bar

Runs a command (via `"${SHELL}" -c`) with a simulated activity bar — progress accelerates early, slows near 90%, and only reaches 100% once the process actually exits. Not a true byte-accurate progress meter.

```bash
frobulator.bar sleep 5
frobulator.bar apt-get update
frobulator.bar rsync -av source/ destination/
```

### frobulator.temporary

Creates one or more temporary directories (template `frobulator.temporary.XXXXXX`, via `mktemp -d`) and, for each, `eval`-assigns the resulting path back into the *named variable you pass in* — so the argument is a variable name, not a path. Several variable names can be passed; the caller's own variables are left untouched.

```bash
frobulator.temporary directory_temporary
# ${directory_temporary} now holds the generated path
```

### frobulator.trap

Registers `EXIT`/`HUP`/`INT`/`PIPE`/`QUIT`/`TERM` traps that recursively delete the given directories on interruption or normal exit. Paths are fixed when the trap is set, and directories from every call are kept - so several temporary directories can be cleaned up. Use immediately after `frobulator.temporary`.

```bash
frobulator.temporary directory_temporary
frobulator.trap "${directory_temporary}"
```

### frobulator.complete

One-line status check after a command, with `frobulator.result` once at the end of the script. The first argument picks what a failure means:

- **`continue`** (default, also when omitted) - records the status, reports `Complete - <label>` or `Incomplete - <label>`, logs failures, and carries on.
- **`halt`** - same on success; on failure prints the message (or `Incomplete - <label>`), logs it, prints the `frobulator.result` summary and exits the script - also when called inside a function.

An optional message and detail follow the keyword; the message replaces the label on failure. Without a message the label is the calling function's name (`image_download` → `image download`). At script level it is the previous command in the script (`frobulator.write` → `write`, `cd` → `cd`), or the action of `frobulator.progress` after a background command (`clone`); assignments and lines it cannot read fall back to `Operation N`. Failures are written to the script's log (`~/.local/var/log/<script>-<stamp>.log`) through `frobulator.log`.

```bash
apk_tool_update
frobulator.complete halt

zipalign -p -f 4 "${file_unsigned}" "${file_aligned}"
frobulator.complete halt "Align failed" "[ ${file_aligned} ]"

rm -f "${cache}"
frobulator.complete

frobulator.result "${script}"
```

The checkpoint form runs a command once and skips it on later runs: **`frobulator.complete [ continue | halt ] "[path]" "[checkpoint]" command [arguments...]`** - skips the command if `"${path}/${checkpoint}"` exists, otherwise runs it, records its status and creates the checkpoint file on success. On failure the stale checkpoint is removed; `halt` then prints the summary and exits.

```bash
frobulator.complete "${checkpoint_directory}" "make-all" make all

frobulator.complete halt "${checkpoint_directory}" "image-checkpoint" download_image
```

### frobulator.halt

Stops the script when the command that ran immediately before it failed - the stop behind `frobulator.complete halt`, also callable directly - a one-line replacement for an `if [ "${?}" -ne 0 ]` block ending in `exit`. On success it records the status for `frobulator.result` and continues silently. On failure it prints the optional message with `frobulator.err`, writes a dated line to the script's log through `frobulator.log` (`~/.local/var/log/<script>-<stamp>.log`), and exits with the failed command's own status.

```bash
zipalign -p -f 4 "${file_unsigned}" "${file_aligned}"
frobulator.halt "Align failed" "[ ${file_aligned} ]"
```

```bash
mkdir -p "${directory}"
frobulator.halt
```

### frobulator.fail

Same as `frobulator.halt`, for use inside functions: instead of exiting the script it returns the failed status, so the caller keeps control. Follow it with `|| return` to leave the calling function - a function cannot make its caller return on its own.

```bash
build () {
	make all
	frobulator.fail "Build failed" "[ ${target} ]" || return
}
```

### frobulator.track

One-line replacement for an `if [ "${?}" -ne 0 ]; then status=1; fi` block inside a function: records the failure of the command that ran immediately before it in the calling function's `status`, and returns that command's own exit status - so it can be chained to leave a loop or the function.

```bash
build () {
	local status=0
	make all
	frobulator.track
	make install
	frobulator.track || return 1
	return "${status}"
}
```

### frobulator.result

Evaluates every status recorded by `frobulator.complete` since the last call, reports overall success or a failure count (pointing at the log directory on failure: `${HOME}/.local/var/log/`, or `${PREFIX}/var/log/` for a system-context run - the same directory `frobulator.log` writes to), then clears the recorded checkpoint/status collections. Use in tandem with `frobulator.complete`.

```bash
frobulator.result "setup"
```

## filesystem helpers

### frobulator.directory

Creates a directory (and sets ownership via `frobulator.ownership`) from a path/name or array. With a single argument containing a `/`, the path is split automatically (parent directory vs. final segment) — so a single absolute path works correctly here, unlike some of the item-based helpers below.

```bash
frobulator.directory "${HOME}/.config" "frobulator"
```

```bash
frobulator.directory "${HOME}/.config/frobulator"
```

### frobulator.write

Writes `content` to one or more files under `path` (`path` defaults to `${PWD}` when a 3-argument call is used), creating the directory first if needed, and sets ownership on each written file. `mode` controls how content is applied:

- `append` — adds content to the end of the file
- `write` — overwrites the file entirely
- `prepend` — adds content before the file's existing contents

```bash
frobulator.write "append" "enabled=true" "${HOME}/.config/frobulator" "config"
```

### frobulator.file

Creates one or more empty files (via `touch`) under `path` (`path` defaults to `${PWD}` when a single-argument call is used) and sets ownership on each.

```bash
frobulator.file "${HOME}/.config/frobulator" "config"
```

### frobulator.keep

Reverse-selects: `cd`s into `path`, then deletes every item in that directory *except* the file(s)/array you list — the inverse of `frobulator.delete`.

```bash
frobulator.keep "${HOME}/Downloads" "important.zip"
```

### frobulator.delete

Deletes selected item(s) from a directory. A single argument is treated as an item relative to `${PWD}` (unlike `frobulator.directory`, it does **not** auto-split a `/`-containing single argument into path + item — pass the base path and item separately, or use `dirname`/`basename`, when deleting an absolute path). Two arguments treat the first as the base path and the second as the item or array of items. Warns instead of silently succeeding when the resolved target doesn't exist.

```bash
frobulator.delete "${temporary_directory}" "old-file.tmp"
```

```bash
list=( adb fastboot )
frobulator.delete "${directory_android}" list
```

### frobulator.copy

Recursively copies file(s)/directories: `frobulator.copy "[source]" "[target]" "[file]" | "[array]"`. Creates `target` if missing.

```bash
frobulator.copy "${source_directory}" "${target_directory}" "config"
```

### frobulator.move

Moves file(s)/directories, then sets `public execute` permissions (`755`) on the moved item(s) at the target via `frobulator.permissions`. `source` defaults to `${PWD}` per item if left empty.

```bash
frobulator.move "${source_directory}" "${target_directory}" "archive.tar.gz"
```

### frobulator.link

Creates symbolic links, detecting and labeling whether each source item is a `file` or `directory` in its status output, then sets ownership on the created link. Three call forms:

- `frobulator.link "[source]" "[target]" "[item]"` — link a single item, same name at target
- `frobulator.link "[source]" "[target]" "[item]" "[link name]"` — link a single item under a different name
- `frobulator.link "[source]" "[target]" "[array]"` (more than 4 total arguments) — link every item in the array, same names

```bash
frobulator.link "${directory_platform_tools}" "${HOME}/.local/bin" "adb"
```

### frobulator.image

Displays a local or remote image directly in supported terminals (local file path, `http(s)://` URL, or stdin). Width defaults to terminal width and cells/percentages are accepted; specifying an explicit height disables automatic aspect-ratio preservation.

```bash
frobulator.image "${image_file}"
frobulator.image "${image_file}" "50%"
frobulator.image "${image_file}" "80" "40"
```

## network helpers

### frobulator.http

Fetches the HTTP status code for a URL into `${status_url}` — silently, via `curl --write-out`. Called with one argument it checks the URL as-is; called with two (`url`, `data`) it checks `"${url}/${data}"` following redirects. Use before `frobulator.status`.

```bash
frobulator.http "https://example.com/file.tar.gz"
```

### frobulator.status

Interprets `${status_url}` (set by a prior `frobulator.http` call) into a 1xx/2xx/3xx/4xx/5xx category, prints a colored status line accordingly, and sets `${proceed}` to `1` (ok to continue) or `0` (abort) plus `${reason}` (`client`/`server`/`response`) on failure.

```bash
frobulator.http "https://example.com/file.tar.gz"
frobulator.status
```

### frobulator.repository

Lists the file names at the top level of a GitHub repository through its contents listing (`curl` only - names extracted with `grep` and `cut`). An optional prefix keeps only names starting with it. Names are stored in the `frobulator_return` array (reset on each call); an unreachable listing or no matching name is an error and makes the function return `1`. Pair with `frobulator.download` to fetch the listed files.

```bash
frobulator.repository "nathaneltitane/terminal" "bash-function-"

frobulator.download get.trmnl.me "${HOME}"/.local/bin "${frobulator_return[@]}"
```

### frobulator.download

Downloads file(s), one request per item, reported by the HTTP status code of the download itself (`2xx` is success; any other code, or an unreachable server, is an error and makes the function return `1`). Each file is written to a partial file beside the target and only moved into place on success, then set to `public execute` (`755`) via `frobulator.permissions`. Missing directories are created. The caller's arrays are left untouched.

Call forms:

- `"[url]" "[directory]" "[item]" | "[array]"` - each item from `"[url]"/"[item]"` into the directory. A single item is fetched from the url itself when the url already ends with the item or with a file name (extension) - so the item becomes the saved name.
- `"[url]"/"[item]" "[directory]"/"[item]"` - the url is the file, saved under the given path.
- `"[url]"/"[item]" "[directory]"` - the url is the file, saved under its own name.
- `"[url]" "[source item]" "[directory]" "[item]"` - `"[url]"/"[source item]"` saved under a different name.

Every message ends its bracket with `// <status code>` - `000` when no response was received. A server that does not answer within 30 seconds is reported as unreachable.

```bash
frobulator.download get.frbltr.app "${HOME}/.local/bin/frobulator"
```

```bash
list=( "dextop" "dextop-additions" )
frobulator.download get.dxtp.app "${HOME}/.local/bin" "${list[@]}"
```

```bash
frobulator.download "https://dl.winehq.org/wine/wine-mono/7.4.0/wine-mono-7.4.0-x86.msi" "${HOME}/Downloads" wine-mono.msi
```

### frobulator.upload

Uploads data or file(s), one request per item. First argument is the HTTP request method (`POST`, `PUT`, etc.), second is the URL, remaining argument(s)/array are payloads — a `{...}` or `[...]` JSON string is sent with a JSON content type, an `@file` argument is sent as multipart form data, anything else is sent as URL-encoded form data. Each upload runs with a `frobulator.progress "upload"` indicator and is reported by the HTTP status code of the upload itself (`2xx` is success; any other code, or an unreachable server, is an error and makes the function return `1`). Every message ends its bracket with `// <status code>` - `000` when no response was received (unreachable server - no answer within 30 seconds - or the request was never sent). The caller's arrays are left untouched.

```bash
frobulator.upload POST "https://example.com/upload" "${archive_file}"
```

```bash
frobulator.upload POST "https://example.com/upload" '{"key":"value"}'
```

```bash
list=( "name=one" "name=two" )
frobulator.upload PUT "https://example.com/items" "${list[@]}"
```

## command wrappers

### frobulator.silence

Runs a command (via `"${SHELL}" -c`) with stdout/stderr redirected to the null sink, for fully silent execution.

```bash
frobulator.silence "apt-get update"
```

### frobulator.log

Runs a command the same way, but redirects output to a timestamped log file instead of discarding it — `${directory_log}/${script}-${stamp}.log`, where `directory_log` is `${PREFIX}/var/log` for a system-context run or `${HOME}/.local/var/log` otherwise.

```bash
frobulator.log "apt-get install curl"
```

## user input helpers

### frobulator.password

Captures masked password input character-by-character, showing `•` per keystroke and handling backspace/delete, until Enter. Result is stored in `${password}` (and returned via `${input}` as well).

```bash
frobulator.password
```

### frobulator.input

Captures one or more prompted values by label. Labels containing `password` are captured with `frobulator.password`; everything else uses a plain `read`. Re-prompts on empty input. Results accumulate in the `frobulator_return` array as `label=value` pairs.

```bash
options=( "username" "password" )
frobulator.input "${options[@]}"
```

## package helpers

*(Debian/`apt`-based; several call `sudo`-equivalent commands directly and expect root or passwordless sudo)*

### frobulator.clean

Runs `apt-get autoremove`, `autoclean`, and `clean` in sequence (each shown with its own progress indicator), then clears stale `dpkg` post-install scripts under `${PREFIX}/var/lib/dpkg/info` to avoid package configuration errors.

```bash
frobulator.clean
```

### frobulator.hold

Marks package(s) for version freeze via `apt-mark hold`.

```bash
frobulator.hold "firefox-esr"
```

### frobulator.release

Reverses `frobulator.hold` via `apt-mark unhold`.

```bash
frobulator.release "firefox-esr"
```

### frobulator.failsafe

Runs a background `apt update && apt full-upgrade`, then sequentially updates/upgrades/installs each named package — a heavier pre-flight pass meant to avoid "not found" or "ignored" errors on the install(s) that follow.

```bash
frobulator.failsafe "curl"
```

### frobulator.install

Installs package(s). A `.deb` path is installed directly; otherwise the package is looked up with `apt search` first, and installation is skipped (with a "present on system" message) if it's already installed.

```bash
frobulator.install "curl"
```

### frobulator.require

Checks whether each name is available as a command or as an installed package (`dpkg-query`), and if neither, searches `apt-file` to find and install the package that provides it. `apt-file` is only installed and refreshed when something is actually missing, so requirements that are already met need no root access — supports `*` glob package-name queries too, expanding them against `apt-cache pkgnames` before installing every match.

```bash
frobulator.require "curl"
```

```bash
frobulator.require "libssl*"
```

### frobulator.reinstall

Reinstalls package(s) via `apt-get install --reinstall`.

```bash
frobulator.reinstall "curl"
```

### frobulator.update

Runs `apt-get update` with a progress indicator.

```bash
frobulator.update
```

### frobulator.upgrade

Runs `apt-get dist-upgrade` with a progress indicator.

```bash
frobulator.upgrade
```

### frobulator.purge

Purges package(s) via `apt-get purge --autoremove`, but only if `apt search` shows the package as currently installed (skips with a message otherwise).

```bash
frobulator.purge "unused-package"
```

### frobulator.dialog

Opens a native directory-selection dialog via `zenity` (GNOME) or `kdialog` (KDE), whichever is available, with `${script}` in the window title, and prints the selection (one path per line) for capture. Extra arguments are passed on to the dialog tool. Fails with a warning if neither is installed.

```bash
directory="$(frobulator.dialog "Directory")"
```

## process, privilege, and result helpers

### frobulator.terminate

Forcefully and repeatedly `pkill -f`'s a process name/pattern until `pgrep -f` no longer finds it. Requires both `pgrep` and `pkill`.

```bash
frobulator.terminate "rogue-process"
```

### frobulator.exit

Cleanly exits the current script or process instance — runs a 3-second `frobulator.countdown` ("Exiting"), then calls the `exit` builtin with an explicit exit code (defaults to `0`, or `1` if the countdown itself was interrupted). Does not touch the shell — see `frobulator.close` for that behavior. Being terminal, it never `return`s to its caller like the rest of the library does.

```bash
frobulator.exit "setup"
```

```bash
frobulator.exit "setup" "${status}"
```

### frobulator.close

Runs the same 3-second `frobulator.countdown`, then forcefully terminates the current `${SHELL}` via `frobulator.terminate` — this ends the shell session, not just the calling script. (This is what `frobulator.exit` used to do before the two were split.)

```bash
frobulator.close "setup"
```

### frobulator.user

Prints the active shell user (`${SUDO_USER}` if set, otherwise `${USER}`) and pauses 1 second — useful before privilege-sensitive steps.

```bash
frobulator.user
```

### frobulator.assess

Checks whether the listed command(s) exist. If run as root, installs any missing ones via `frobulator.require` and asks the person to restart as their normal user. If not root, prompts to escalate (`frobulator.escalate`) for any missing command, or warns and fails if declined.

```bash
requirements=( curl git )
frobulator.assess "${requirements[@]}"
```

### frobulator.escalate

Relaunches the current script as root via `sudo`, preserving the original arguments (read back from the `self_arguments` array set by the bootstrap header - spaces, quotes and empty arguments are kept exactly; a plain string from older headers is still word split). If already root, resolves the correct non-root `USER`/`HOME` from `SUDO_USER` instead of re-launching. Before restarting it clears the bootstrap header and its own messages (3 header lines + `lines_printed_terminal`); the restarted run then clears its own header and the sudo password prompt, so only the escalated runtime messages stay on screen.

```bash
self_arguments=("${@}")
frobulator.escalate
```

### frobulator.self

Declares frobulator as loaded in the current shell by setting `frobulator_loaded="true"` - the bootstrap header checks it to skip downloading and sourcing again. Runs automatically when frobulator is sourced.

```bash
frobulator.self
```

### frobulator.service

Manages a systemd service, scoping automatically to `--user` or system context depending on whether the runtime is escalated (root). A single argument is treated as a daemon-level action with no target unit; two arguments target a specific service with the given action forwarded to `systemctl`. `activate` runs a full enable/start cycle with status verification. `reload` is the only action that doesn't require a service. Supported actions: `reload`, `status`, `activate`, `enable`, `disable`, `start`, `stop`, `restart`.

```bash
frobulator.service reload
```

```bash
frobulator.service reload "${service}"
```

```bash
frobulator.service activate "${service}"
```

### frobulator.ownership

Restores ownership on a target after privileged operations. Resolves the real path and classifies it: paths under the invoking user's home directory are attributed to that user; paths under system directories (`/root`, `/usr`, `/etc`, `/var`, `/opt`, `/boot`) are attributed to root; anything else falls back to the invoking user. Applied recursively via `chown`. Called internally by `frobulator.directory`, `frobulator.write`, `frobulator.file`, and `frobulator.link`.

```bash
frobulator.ownership "${directory_android}" "${HOME}/.local/bin/adb"
```

### frobulator.permissions

Sets file / directory permissions from a scope and optional modifiers, in place of raw `chmod` modes. Takes the scope first, then any modifiers (any order), then one or more targets (or an array). Sets the exact mode; directories always keep `execute` for whoever can read them, since a directory cannot be opened without it.

- **scope:** `public` (you edit, everyone reads), `group` (you edit, your group reads, no access for others), `private` (only you)
- **modifiers:** `execute` (whoever can read can also run), `readonly` (nobody edits, including you)

| call | file | directory | typical use |
|---|---|---|---|
| `public` | `644` `rw-r--r--` | `755` `rwxr-xr-x` | regular files, configs, shared data |
| `public execute` | `755` `rwxr-xr-x` | `755` `rwxr-xr-x` | scripts, binaries, installed tools |
| `public readonly` | `444` `r--r--r--` | `555` `r-xr-xr-x` | reference files that should not change |
| `public readonly execute` | `555` `r-xr-xr-x` | `555` `r-xr-xr-x` | locked-down tools |
| `group` | `640` `rw-r-----` | `750` `rwxr-x---` | files for your group only |
| `group execute` | `750` `rwxr-x---` | `750` `rwxr-x---` | group-only scripts |
| `group readonly` | `440` `r--r-----` | `550` `r-xr-x---` | group reference files |
| `group readonly execute` | `550` `r-xr-x---` | `550` `r-xr-x---` | locked group tools |
| `private` | `600` `rw-------` | `700` `rwx------` | credentials, keys, tokens |
| `private execute` | `700` `rwx------` | `700` `rwx------` | personal scripts |
| `private readonly` | `400` `r--------` | `500` `r-x------` | ssh keys, certificates |
| `private readonly execute` | `500` `r-x------` | `500` `r-x------` | locked personal tools |

```bash
frobulator.permissions public execute "${path}/${file}"
```

```bash
frobulator.permissions private "${HOME}/.git-credentials"
```

```bash
frobulator.permissions private readonly "${HOME}/.ssh/id_ed25519"
```

## archive helpers

### frobulator.archive

Creates an archive from a directory (defaults to `${PWD}` when omitted). Supported `type` values: `tar`, `tar.gz`/`tgz`, `tar.bz2`/`tbz2`, `zip`, `7z`, `rar` — each checked and installed via `frobulator.require` before use.

```bash
frobulator.archive "backup" "tar.gz" "${HOME}/Documents"
```

### frobulator.extract

Extracts a known archive type (detected from everything after the first period of the file name, so names like `backup-1.2.tar.gz` are not recognized) into `directory` (defaults to `${PWD}` when omitted), installing the needed extractor (`tar`, `7z`, `unrar`, `unzip`) via `frobulator.require` first.

```bash
frobulator.extract "backup.tar.gz" "${target_directory}"
```

## complete command index

```text
frobulator.plo
frobulator.pmt
frobulator.brk
frobulator.nil
frobulator.inf
frobulator.wrn
frobulator.msg
frobulator.add
frobulator.rem
frobulator.ret
frobulator.rel
frobulator.fwd
frobulator.rev
frobulator.stp
frobulator.dwl
frobulator.upl
frobulator.lnk
frobulator.scs
frobulator.err
frobulator.ins
frobulator.cpt
frobulator.url
frobulator.qst
frobulator.ipt
frobulator.usr
frobulator.nul
frobulator.ind
frobulator.ltr
frobulator.num
frobulator.sep
frobulator.ntf
frobulator.columns
frobulator.black
frobulator.silver
frobulator.grey
frobulator.white
frobulator.red
frobulator.crimson
frobulator.green
frobulator.lime
frobulator.yellow
frobulator.orange
frobulator.blue
frobulator.navy
frobulator.magenta
frobulator.purple
frobulator.fuschia
frobulator.pink
frobulator.aqua
frobulator.teal
frobulator.erase
frobulator.separate
frobulator.read
frobulator.script
frobulator.type
frobulator.timeout
frobulator.clear
frobulator.countdown
frobulator.action
frobulator.process
frobulator.progress
frobulator.bar
frobulator.temporary
frobulator.trap
frobulator.complete
frobulator.halt
frobulator.fail
frobulator.track
frobulator.result
frobulator.ownership
frobulator.permissions
frobulator.directory
frobulator.write
frobulator.file
frobulator.keep
frobulator.delete
frobulator.copy
frobulator.move
frobulator.link
frobulator.image
frobulator.http
frobulator.status
frobulator.download
frobulator.upload
frobulator.silence
frobulator.log
frobulator.password
frobulator.input
frobulator.clean
frobulator.hold
frobulator.release
frobulator.failsafe
frobulator.install
frobulator.require
frobulator.reinstall
frobulator.update
frobulator.upgrade
frobulator.purge
frobulator.dialog
frobulator.exit
frobulator.terminate
frobulator.close
frobulator.user
frobulator.assess
frobulator.escalate
frobulator.self
frobulator.service
frobulator.archive
frobulator.extract
```

### Uses:

The following projects incorporate Frobulator in their usage:

[[ Dextop // Project Page ]](https://github.com/nathaneltitane/dextop)

[[ L²CU // Project Page ]](https://github.com/nathaneltitane/l2cu)

[[ Terminal // Project Page ]](https://github.com/nathaneltitane/terminal)

[[ Mecha // Blocks // Project Page ]](https://github.com/nathaneltitane/mechablocks)

[[ Nathanel + Titane // Project Page ]](https://github.com/nathaneltitane/nathaneltitane)

### Repositories:

[GNU/Bash](https://github.com/gitGNU/gnu_bash) as the shell environment on top of which the scripts function.

### Reports:

[Submit bug report or feature request](https://github.com/nathaneltitane/terminal/issues)

### Projects:

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/dextop?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=DEXTOP)](https://github.com/nathaneltitane/dextop)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/frobulator?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=FROBULATOR)](https://github.com/nathaneltitane/frobulator)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/gutengrab?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=GutenGrab)](https://github.com/nathaneltitane/gutengrab)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/l2cu?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=L²CU)](https://github.com/nathaneltitane/l2cu)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/terminal?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=TERMINAL)](https://github.com/nathaneltitane/terminal)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/mechablocks?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=MECHA%20//%20BLOCKS)](https://github.com/nathaneltitane/mechablocks)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/pixtrm?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=PIXTRM)](https://github.com/nathaneltitane/pixtrm)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/nathaneltitane?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=NATHANEL%20%2b%20TITANE)](https://github.com/nathaneltitane/nathaneltitane)

[![GitHub Repo stars](https://img.shields.io/github/stars/nathaneltitane/pewpewprints?style=for-the-badge&logo=gnubash&logoColor=ffffff&label=PEW%21%20PEW%21%20PRINTS)](https://github.com/nathaneltitane/pewpewprints)

---

[[ Frobulator // Project Page ]](https://github.com/nathaneltitane/frobulator) [ Version // 2026-10-02 ]

### Enjoying Frobulator? Buy me a coffee to show your appreciation!

[![Donate](https://img.shields.io/badge/Paypal-2f343f.svg?style=for-the-badge&logo=paypal&label=Donate)](https://www.paypal.com/donate?hosted_button_id=ZW3CDCANHJCWJ)
