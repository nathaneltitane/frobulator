![frobulator](https://raw.githubusercontent.com/nathaneltitane/frobulator/main/frobulator.svg)

[![Donate](https://img.shields.io/badge/Paypal-2f343f.svg?style=for-the-badge&logo=paypal&label=Donate)](https://www.paypal.com/donate?hosted_button_id=ZW3CDCANHJCWJ)

[[ Frobulator // Project Page ]](https://github.com/nathaneltitane/frobulator) [ Version // 2026-09-04 ]

---

### Welcome to [Frobulator](https://frobulator.app)

Frobulator is a custom shell parser and scripting function library: Frobulate all the things!

Frobulator is easy to use and understand and is meant to help streamline your shell scripting projects while providing you with:

- Colorized prompts
- Line header markers for various message types
- Intractive counters and timers
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
   - Script checkpoint solutions to interate over only failed elements or modules
   - Streamlined archive detection and extraction routines
   - Clean ogging, redirection and silencing functions for pretty execution and informed debugging

**...all while making redundant and complex code bits a thing of the past!**

### Note:

The current set of assertions upon which Frobulator is built restricts its functionality to scripts exclusively, at least for the time being.

### Usage:

## source

```bash
. "${HOME}"/.local/bin/frobulator
```

## standard script bootstrap

```bash
#!/bin/bash

# dependencies /////////////////////////////////////////////////////////////////

ticker () {

	echo

	for ticker in '>  ' '>> ' '>>>'
	do
		echo -n -e "\r[  ${ticker}  ] ${1^}..."

		sleep 0.5
	done

	echo

	echo

}

if [[ $(id -u -n) = "root" ]]
then
	USER="${SUDO_USER:-root}"

	if [ "${USER}" = "root" ]
	then
		HOME=/root
	else
		HOME="/home/${USER}"
	fi
fi

ticker checking

package_managers_list=(
	"apt-get --quiet --quiet update > /dev/null 2>&1; apt-get --quiet --quiet install --yes"
	"dnf --quiet install --assumeyes"
	"yum --quiet install --assumeyes"
	"apk add --quiet"
	"pacman --sync --refresh --noconfirm --noprogressbar"
	"zypper --quiet --non-interactive install"
)

package="curl"

if [[ -z $(command -v "${package}") ]]
then
	for package_manager in "${package_managers_list[@]}"
	do
		entry="${package_manager%% *}"

		if [[ -n $(command -v "${entry}") ]]
		then
			eval "${package_manager} ${package} > /dev/null 2>&1"

			break
		fi
	done

	if [[ -z $(command -v "${package}") ]]
	then
		echo "[  !  ] Unable to install or binary not found /////////////////////// [ '${package}' ]"
		echo

		exit 1
	fi
fi

mkdir -p "${HOME}"/.local/bin

frobulator="${HOME}"/.local/bin/frobulator

version_online=$(curl -s -L get.frbltr.app | grep -m 1 '^# version=' | cut -d '"' -f 2)
version_local=$(grep -m 1 '^# version=' "${frobulator}" 2>/dev/null | cut -d '"' -f 2)

version_local="${version_local:-1970-01-01}"

if [ ! -f "${frobulator}" ] || [[ "${version_online}" > "${version_local}" ]]
then
	curl -s -L get.frbltr.app > "${frobulator}"

	chmod +x "${frobulator}"
fi

ticker initializing

source "${frobulator}"

# script ///////////////////////////////////////////////////////////////////////

script=$(basename -- "${BASH_SOURCE[0]}")

# version //////////////////////////////////////////////////////////////////////

version="MM-DD-YY"

# usage ////////////////////////////////////////////////////////////////////////

# prompt ///////////////////////////////////////////////////////////////////////

frobulator.script "Setting up ${script#*-}"

# variables ////////////////////////////////////////////////////////////////////

# defaults /////////////////////////////////////////////////////////////////////

# functions ////////////////////////////////////////////////////////////////////
```
## prompt formatting

### frobulator.plo

clears a defined number of lines above the current cursor position, wiping from that point down to the end of the screen — built on `terminal_cursor_up` and `terminal_clear_down`.

```bash
frobulator.plo 1
```

### frobulator.erase

thin wrapper around `frobulator.plo` — erases a defined number of lines above the current cursor position the same way.

```bash
frobulator.erase 3
```

### frobulator.pmt

the shared prompt-formatting engine behind every color and marker command. Normalizes 1–3 arguments (`begin`, `end`, `span character`) into an 80-column line: pads/fills the middle span, folds `begin` with word-detection when it would overflow the line, and truncates an overlong `end` with an ellipsis while preserving its surrounding bracket style. Populates the `prompt_string` array consumed by every wrapper below rather than printing directly.

```bash
frobulator.pmt "Downloading" "[ package.tar.gz ]"
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

## prompt marker commands

These commands print standard frobulator markers by calling `frobulator.pmt` directly and prefixing its own colored marker glyph (e.g. `[  i  ]`, `[  !  ]`). Most accept a message, an optional detail string, and an optional fill character.

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
| `frobulator.ask` | question prompt (no newline) | `frobulator.ask "Enter value"` |
| `frobulator.ipt` | input prompt (no newline) | `frobulator.ipt "Enter value"` |
| `frobulator.usr` | user prompt (no newline) | `frobulator.usr "Enter value"` |
| `frobulator.nul` | continue line, retain color | `frobulator.nul "continued output"` |
| `frobulator.ind` | continue line, clear color | `frobulator.ind "continued output"` |

## argument handling

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
[ i ] Checking dependencies /////////////////////////////////////////// [ curl ]
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

## structured prompt helpers

### frobulator.ltr

prints a lettered step marker — first argument is the letter, remaining arguments are forwarded to `frobulator.pmt`.

```bash
frobulator.ltr "a" "Select source directory"
```

### frobulator.num

prints a numbered step marker — first argument is the number, remaining arguments are forwarded to `frobulator.pmt`.

```bash
frobulator.num "1" "Install dependencies"
```

### frobulator.sep

prints a full-width separator line built from `frobulator.pmt`.

```bash
frobulator.sep
```

### frobulator.ntf

prints a framed notice block. Optional leading arguments select a frame `style` (`square` [default], `round`, `heavy`, `double`, `dots`, `matrix`, `tech`, `skel`, `ascii`), the `split` keyword (renders title and message as two separate frames instead of one divided frame), and a marker-type keyword (`inf`, `wrn`, `scs`, `err`, etc.) to color the frame using that marker's color. Remaining arguments are `title` then `message`.

```bash
frobulator.ntf round inf "Notice" "The setup process is ready."
```

```bash
frobulator.ntf heavy split err "Failure" "Could not reach the update server."
```

### frobulator.separate

prints a predefined `frobulator.sep` line followed by a blank line — use to separate instructions or warnings from prompts.

```bash
frobulator.separate
```

### frobulator.read

wraps the `read` builtin (forwarding all arguments to it) and prints a trailing blank line, so prompts stay evenly spaced after user input.

```bash
frobulator.ask "Continue?" "[ y/n ]"
frobulator.read reply
```

### frobulator.script

prints a script startup banner. Derives the displayed script name/version by splitting `${script}` on its first `-` character (i.e. the running script should be named like `setup-myproject`), falling back to the full script name when no `-` is present.

```bash
frobulator.script "Setting up ${script#*-}"
```

### frobulator.type

prints a string one character at a time with a randomized delay between each, to emulate human typing. First argument is the string, second argument is the maximum random interval in tenths of a second (defaults to `2`, i.e. up to ~0.2s per character).

```bash
frobulator.type "Preparing environment..." 3
```

### frobulator.timeout

sleeps for a number of seconds (defaults to `1` when omitted) — a simple pause between commands.

```bash
frobulator.timeout 5
```

### frobulator.clear

pauses for `frobulator.timeout`'s default interval, then clears the terminal — used for script "paging" once a step's checkpoints are met.

```bash
frobulator.clear
```

### frobulator.countdown

shows a live countdown before continuing, validating that the first argument is a non-negative integer. Remaining arguments are forwarded to `frobulator.pmt` as the message shown beside the counter.

```bash
frobulator.countdown 10 "Starting install" "[ press ctrl+c to cancel ]"
```

## process and progress helpers

### frobulator.action

generates a randomly colored, grammatically conjugated action prompt from a verb (e.g. `download` → `Downloading...`, `panic` → `Panicking...`), falling back to `frobulate` when no verb is given. Handles common English suffix rules (`-ie` → `-y`, silent `-e` drop, consonant doubling, `-c` → `-ck`) before appending `-ing`. Sets the `progress_prompt`/`progress_color` globals consumed by `frobulator.progress`. Callable directly, but normally invoked internally.

```bash
frobulator.action "download" "[ ${file} ]"
```

### frobulator.process

waits on the most recently backgrounded process (`${!}`), animating a simple `/\` ticker beside a message until it exits, then returns that process's exit status.

```bash
apt-get update &
frobulator.process "Updating package index"
```

### frobulator.progress

same as `frobulator.process`, but generates its message via `frobulator.action` (so the first argument is a bare verb, not a pre-built message) and animates a smoother `⎺⎻⎼⎽⎼⎻` ticker.

```bash
curl -L "${url}" -o "${file}" &
frobulator.progress "download" "[ ${file} ]"
```

### frobulator.bar

runs a command (via `"${SHELL}" -c`) with a simulated activity bar — progress accelerates early, slows near 90%, and only reaches 100% once the process actually exits. Not a true byte-accurate progress meter.

```bash
frobulator.bar sleep 5
frobulator.bar apt-get update
frobulator.bar rsync -av source/ destination/
```

### frobulator.temporary

creates one or more temporary directories (template `frobulator.temporary.XXXXXX`, via `mktemp -d`) and, for each, `eval`-assigns the resulting path back into the *named variable you pass in* — so the argument is a variable name, not a path.

```bash
frobulator.temporary directory_temporary
# ${directory_temporary} now holds the generated path
```

### frobulator.trap

registers `EXIT`/`HUP`/`INT`/`PIPE`/`QUIT`/`TERM` traps that recursively delete the named temporary directory on interruption or normal exit. Use immediately after `frobulator.temporary`.

```bash
frobulator.temporary directory_temporary
frobulator.trap directory_temporary
```

### frobulator.complete

Two modes:

- **No arguments** — captures the exit status of the command that ran immediately before it and records it as an anonymous "Operation N", printing complete/incomplete accordingly. Covers what a separate `frobulator.continue` command used to do; that command no longer exists.
- **`frobulator.complete "[path]" "[checkpoint]" command [arguments...]`** — skips running the command if `"${path}/${checkpoint}"` already exists (checkpoint already satisfied); otherwise runs it, records its status, and creates the checkpoint file on success (removing any stale checkpoint file on failure).

Every call's status/checkpoint pair accumulates in memory for `frobulator.result` to evaluate afterward.

```bash
make all
frobulator.complete
```

```bash
frobulator.complete "${checkpoint_directory}" "make-all" make all
```

### frobulator.result

evaluates every status recorded by `frobulator.complete` since the last call, reports overall success or a failure count (pointing at `${PREFIX}/var/log/` for details on failure), then clears the recorded checkpoint/status collections. Use in tandem with `frobulator.complete`.

```bash
frobulator.result "setup"
```

## filesystem helpers

### frobulator.directory

creates a directory (and sets ownership via `frobulator.ownership`) from a path/name or array. With a single argument containing a `/`, the path is split automatically (parent directory vs. final segment) — so a single absolute path works correctly here, unlike some of the item-based helpers below.

```bash
frobulator.directory "${HOME}/.config" "frobulator"
```

```bash
frobulator.directory "${HOME}/.config/frobulator"
```

### frobulator.write

appends `content` to one or more files under `path` (`path` defaults to `${PWD}` when a 2-argument call is used), creating the directory first if needed, and sets ownership on each written file.

```bash
frobulator.write "enabled=true" "${HOME}/.config/frobulator" "config"
```

### frobulator.flag

same as `frobulator.write`, but overwrites the file instead of appending, and additionally sets `a+rx` permissions after writing — useful for checkpoint flags.

```bash
frobulator.flag "ready" "${temporary_directory}" "checkpoint"
```

### frobulator.file

creates one or more empty files (via `touch`) under `path` (`path` defaults to `${PWD}` when a single-argument call is used) and sets ownership on each.

```bash
frobulator.file "${HOME}/.config/frobulator" "config"
```

### frobulator.keep

reverse-selects: `cd`s into `path`, then deletes every item in that directory *except* the file(s)/array you list — the inverse of `frobulator.delete`.

```bash
frobulator.keep "${HOME}/Downloads" "important.zip"
```

### frobulator.delete

deletes selected item(s) from a directory. A single argument is treated as an item relative to `${PWD}` (unlike `frobulator.directory`, it does **not** auto-split a `/`-containing single argument into path + item — pass the base path and item separately, or use `dirname`/`basename`, when deleting an absolute path). Two arguments treat the first as the base path and the second as the item or array of items. Warns instead of silently succeeding when the resolved target doesn't exist.

```bash
frobulator.delete "${temporary_directory}" "old-file.tmp"
```

```bash
list=( adb fastboot )
frobulator.delete "${path_android}" list
```

### frobulator.copy

recursively copies file(s)/directories: `frobulator.copy "[source]" "[target]" "[file]" | "[array]"`. Creates `target` if missing.

```bash
frobulator.copy "${source_directory}" "${target_directory}" "config"
```

### frobulator.move

moves file(s)/directories, then sets `a+rx` permissions on the moved item(s) at the target. `source` defaults to `${PWD}` per item if left empty.

```bash
frobulator.move "${source_directory}" "${target_directory}" "archive.tar.gz"
```

### frobulator.link

creates symbolic links, detecting and labeling whether each source item is a `file` or `directory` in its status output, then sets ownership on the created link. Three call forms:

- `frobulator.link "[source]" "[target]" "[item]"` — link a single item, same name at target
- `frobulator.link "[source]" "[target]" "[item]" "[link name]"` — link a single item under a different name
- `frobulator.link "[source]" "[target]" "[array]"` (more than 4 total arguments) — link every item in the array, same names

```bash
frobulator.link "${path_android_platform_tools}" "${HOME}/.local/bin" "adb"
```

### frobulator.image

displays a local or remote image directly in supported terminals (local file path, `http(s)://` URL, or stdin). Width defaults to terminal width and cells/percentages are accepted; specifying an explicit height disables automatic aspect-ratio preservation.

```bash
frobulator.image "${image_file}"
frobulator.image "${image_file}" "50%"
frobulator.image "${image_file}" "80" "40"
```

## network helpers

### frobulator.http

fetches the HTTP status code for a URL into `${url_status}` — silently, via `curl --write-out`. Called with one argument it checks the URL as-is; called with two (`url`, `data`) it checks `"${url}/${data}"` following redirects. Use before `frobulator.status`.

```bash
frobulator.http "https://example.com/file.tar.gz"
```

### frobulator.status

interprets `${url_status}` (set by a prior `frobulator.http` call) into a 1xx/2xx/3xx/4xx/5xx category, prints a colored status line accordingly, and sets `${proceed}` to `1` (ok to continue) or `0` (abort) plus `${reason}` (`client`/`server`/`response`) on failure.

```bash
frobulator.http "https://example.com/file.tar.gz"
frobulator.status
```

### frobulator.download

downloads file(s) with URL status verification first. Several call shapes are supported depending on argument count — `"[url]" "[directory]" "[item]"` (or array), a combined `"[url]"/"[item]"` two-argument shorthand, or a 4-argument form that downloads a differently-named source item under a new local name. Runs each download in the background with `frobulator.progress "download"` as the visual indicator, and sets `a+rx` permissions on success.

```bash
frobulator.download "https://get.frbltr.app" "${HOME}/.local/bin" "frobulator"
```

### frobulator.upload

uploads data or file(s) after validating the target URL the same way `frobulator.download` does. First argument is the HTTP request method (`POST`, `PUT`, etc.), second is the URL, remaining argument(s)/array are payloads — a `{...}` JSON string is sent with a JSON content type, an `@file` argument is sent as multipart form data, anything else is sent as URL-encoded form data.

```bash
frobulator.upload POST "https://example.com/upload" "${archive_file}"
```

```bash
frobulator.upload POST "https://example.com/upload" '{"key":"value"}'
```

## command wrappers

### frobulator.silence

runs a command (via `"${SHELL}" -c`) with stdout/stderr redirected to the null sink, for fully silent execution.

```bash
frobulator.silence "apt-get update"
```

### frobulator.log

runs a command the same way, but redirects output to a timestamped log file instead of discarding it — `${log_directory}/${script}-${stamp}.log`, where `log_directory` is `${PREFIX}/var/log` for a system-context run or `${HOME}/.local/var/log` otherwise.

```bash
frobulator.log "apt-get install curl"
```

## user input helpers

### frobulator.password

captures masked password input character-by-character, showing `•` per keystroke and handling backspace/delete, until Enter. Result is stored in `${password}` (and returned via `${input}` as well).

```bash
frobulator.password
```

### frobulator.input

captures one or more prompted values by label. Labels containing `password` are captured with `frobulator.password`; everything else uses a plain `read`. Re-prompts on empty input. Results accumulate in the `frobulator_return` array as `label=value` pairs.

```bash
options=( "username" "password" )
frobulator.input "${options[@]}"
```

## package helpers

*(Debian/`apt`-based; several call `sudo`-equivalent commands directly and expect root or passwordless sudo)*

### frobulator.clean

runs `apt-get autoremove`, `autoclean`, and `clean` in sequence (each shown with its own progress indicator), then clears stale `dpkg` post-install scripts under `${PREFIX}/var/lib/dpkg/info` to avoid package configuration errors.

```bash
frobulator.clean
```

### frobulator.hold

marks package(s) for version freeze via `apt-mark hold`.

```bash
frobulator.hold "firefox-esr"
```

### frobulator.release

reverses `frobulator.hold` via `apt-mark unhold`.

```bash
frobulator.release "firefox-esr"
```

### frobulator.failsafe

runs a background `apt update && apt full-upgrade`, then sequentially updates/upgrades/installs each named package — a heavier pre-flight pass meant to avoid "not found" or "ignored" errors on the install(s) that follow.

```bash
frobulator.failsafe "curl"
```

### frobulator.install

installs package(s). A `.deb` path is installed directly; otherwise the package is looked up with `apt search` first, and installation is skipped (with a "present on system" message) if it's already installed.

```bash
frobulator.install "curl"
```

### frobulator.require

checks whether each named *command* (not package name) is available, and if not, searches `apt-file` to find and install the package that provides it — supports `*` glob package-name queries too, expanding them against `apt-cache pkgnames` before installing every match.

```bash
frobulator.require "curl"
```

```bash
frobulator.require "libssl*"
```

### frobulator.reinstall

reinstalls package(s) via `apt-get install --reinstall`.

```bash
frobulator.reinstall "curl"
```

### frobulator.update

runs `apt-get update` with a progress indicator.

```bash
frobulator.update
```

### frobulator.upgrade

runs `apt-get dist-upgrade` with a progress indicator.

```bash
frobulator.upgrade
```

### frobulator.purge

purges package(s) via `apt-get purge --autoremove`, but only if `apt search` shows the package as currently installed (skips with a message otherwise).

```bash
frobulator.purge "unused-package"
```

### frobulator.dialog

opens a native file-selection dialog via `zenity` (GNOME) or `kdialog` (KDE), whichever is available, with `${script}` in the window title. Fails with a warning if neither is installed.

```bash
frobulator.dialog "Select a directory" --directory
```

## process, privilege, and result helpers

### frobulator.terminate

forcefully and repeatedly `pkill -f`'s a process name/pattern until `pgrep -f` no longer finds it. Requires both `pgrep` and `pkill`.

```bash
frobulator.terminate "rogue-process"
```

### frobulator.exit

cleanly exits the current script or process instance — runs a 3-second `frobulator.countdown` ("Exiting"), then calls the `exit` builtin with an explicit exit code (defaults to `0`, or `1` if the countdown itself was interrupted). Does not touch the shell — see `frobulator.close` for that behavior. Being terminal, it never `return`s to its caller like the rest of the library does.

```bash
frobulator.exit "setup"
```

```bash
frobulator.exit "setup" "${status}"
```

### frobulator.close

runs the same 3-second `frobulator.countdown`, then forcefully terminates the current `${SHELL}` via `frobulator.terminate` — this ends the shell session, not just the calling script. (This is what `frobulator.exit` used to do before the two were split; its internal doc comment still shows the old `frobulator.exit "[instance]"` usage line.)

```bash
frobulator.close "setup"
```

### frobulator.user

prints the active shell user (`${SUDO_USER}` if set, otherwise `${USER}`) and pauses 1 second — useful before privilege-sensitive steps.

```bash
frobulator.user
```

### frobulator.assess

checks whether the listed command(s) exist. If run as root, installs any missing ones via `frobulator.require` and asks the person to restart as their normal user. If not root, prompts to escalate (`frobulator.escalate`) for any missing command, or warns and fails if declined.

```bash
requirements=( curl git )
frobulator.assess "${requirements[@]}"
```

### frobulator.escalate

relaunches the current script as root via `sudo`, preserving the original arguments (read back from the exported `self_arguments` variable). If already root, resolves the correct non-root `USER`/`HOME` from `SUDO_USER` instead of re-launching.

```bash
export self_arguments="${@}"
frobulator.escalate
```

### frobulator.service

manages a systemd service, scoping automatically to `--user` or system context depending on whether the runtime is escalated (root). A single argument is treated as a daemon-level action with no target unit; two arguments target a specific service with the given action forwarded to `systemctl`. `activate` runs a full enable/start cycle with status verification. `reload` is the only action that doesn't require a service. Supported actions: `reload`, `status`, `activate`, `enable`, `disable`, `start`, `stop`, `restart`.

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

restores ownership on a target after privileged operations. Resolves the real path and classifies it: paths under the invoking user's home directory are attributed to that user; paths under system directories (`/root`, `/usr`, `/etc`, `/var`, `/opt`, `/boot`) are attributed to root; anything else falls back to the invoking user. Applied recursively via `chown`. Called internally by `frobulator.directory`, `frobulator.write`, `frobulator.flag`, `frobulator.file`, and `frobulator.link`.

```bash
frobulator.ownership "${path_android}" "${HOME}/.local/bin/adb"
```

## archive helpers

### frobulator.archive

creates an archive from a directory (defaults to `${PWD}` when omitted). Supported `type` values: `tar`, `tar.gz`/`tgz`, `tar.bz2`/`tbz2`, `zip`, `7z`, `rar` — each checked and installed via `frobulator.require` before use.

```bash
frobulator.archive "backup" "tar.gz" "${HOME}/Documents"
```

### frobulator.extract

extracts a known archive type (detected from the file's extension — the source assumes the filename contains no other periods) into `directory` (defaults to `${PWD}` when omitted), installing the needed extractor (`tar`, `7z`, `unrar`, `unzip`) via `frobulator.require` first.

```bash
frobulator.extract "backup.tar.gz" "${target_directory}"
```

## complete command index

```text
frobulator.plo
frobulator.pmt
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
frobulator.ask
frobulator.ipt
frobulator.usr
frobulator.nul
frobulator.ind
frobulator.ltr
frobulator.num
frobulator.sep
frobulator.ntf
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
frobulator.result
frobulator.ownership
frobulator.directory
frobulator.write
frobulator.flag
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

[[ Frobulator // Project Page ]](https://github.com/nathaneltitane/frobulator) [ Version // 2026-09-04 ]

### Enjoying Frobulator? Buy me a coffee to show your appreciation!

[![Donate](https://img.shields.io/badge/Paypal-2f343f.svg?style=for-the-badge&logo=paypal&label=Donate)](https://www.paypal.com/donate?hosted_button_id=ZW3CDCANHJCWJ)
