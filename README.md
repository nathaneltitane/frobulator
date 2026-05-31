![frobulator](https://raw.githubusercontent.com/nathaneltitane/frobulator/main/frobulator.svg)

[![Donate](https://img.shields.io/badge/Paypal-2f343f.svg?style=for-the-badge&logo=paypal&label=Donate)](https://www.paypal.com/donate?hosted_button_id=ZW3CDCANHJCWJ)

[[ Frobulator // Project Page ]](https://github.com/nathaneltitane/frobulator) [ Version // 05-31-2026 ]

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

if [ -f "${HOME}"/.local/bin/frobulator ]
then
	rm -r -f "${HOME}"/.local/bin/frobulator
fi

if [[ -z $(command -v frobulator) ]]
then
	if [[ $(id -u -n) = "root" ]]
	then
		SUDO_HOME=/root

		USER="${SUDO_USER}"

		HOME=/home/"${USER}"
	fi

	if [[ -z $(command -v curl) ]]
	then
		yes | apt-get install curl
	fi

	if [ ! -d "${HOME}"/.local/bin ]
	then
		mkdir -p "${HOME}"/.local/bin
	fi

	curl -s -L get.frbltr.app > "${HOME}"/.local/bin/frobulator

	chmod +x "${HOME}"/.local/bin/frobulator
fi

. "${HOME}"/.local/bin/frobulator
```

## standard script header

```bash
# superuser ////////////////////////////////////////////////////////////////////

export self_arguments="${@}"

frobulator.escalate

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

clears and rewrites prompt output above the current cursor position.

```bash
frobulator.plo 1
```

### frobulator.pmt

normalizes prompt strings, handles span padding, and folds long lines.

```bash
frobulator.pmt "Downloading" "[ package.tar.gz ]"
```

### frobulator.color

generic color engine used by the named color wrappers.

```bash
frobulator.color blue "Building" "[ frobulator ]"
```

## color commands

All color wrappers use the same prompt formatting rules as `frobulator.color`.

| command               | example                                           |
|-----------------------|---------------------------------------------------|
| `frobulator.black`    | `frobulator.black "Highlighted" "[ value ]"`      |
| `frobulator.silver`   | `frobulator.silver "Highlighted" "[ value ]"`     |
| `frobulator.grey`     | `frobulator.grey "Highlighted" "[ value ]"`       |
| `frobulator.white`    | `frobulator.white "Highlighted" "[ value ]"`      |
| `frobulator.red`      | `frobulator.red "Highlighted" "[ value ]"`        |
| `frobulator.crimson`  | `frobulator.crimson "Highlighted" "[ value ]"`    |
| `frobulator.green`    | `frobulator.green "Highlighted" "[ value ]"`      |
| `frobulator.lime`     | `frobulator.lime "Highlighted" "[ value ]"`       |
| `frobulator.yellow`   | `frobulator.yellow "Highlighted" "[ value ]"`     |
| `frobulator.orange`   | `frobulator.orange "Highlighted" "[ value ]"`     |
| `frobulator.blue`     | `frobulator.blue "Highlighted" "[ value ]"`       |
| `frobulator.navy`     | `frobulator.navy "Highlighted" "[ value ]"`       |
| `frobulator.magenta`  | `frobulator.magenta "Highlighted" "[ value ]"`    |
| `frobulator.purple`   | `frobulator.purple "Highlighted" "[ value ]"`     |
| `frobulator.fuschia`  | `frobulator.fuschia "Highlighted" "[ value ]"`    |
| `frobulator.pink`     | `frobulator.pink "Highlighted" "[ value ]"`       |
| `frobulator.aqua`     | `frobulator.aqua "Highlighted" "[ value ]"`       |
| `frobulator.teal`     | `frobulator.teal "Highlighted" "[ value ]"`       |

## prompt marker commands

These commands print standard frobulator markers using their configured colors. Most prompt marker commands accept a message, an optional detail string, and an optional fill character.

| command            | purpose                     | example                                     |
|--------------------|-----------------------------|---------------------------------------------|
| `frobulator.nil`   | empty marker line           | `frobulator.nil "Message" "[ detail ]"`     |
| `frobulator.inf`   | information line            | `frobulator.inf "Message" "[ detail ]"`     |
| `frobulator.wrn`   | warning line                | `frobulator.wrn "Message" "[ detail ]"`     |
| `frobulator.msg`   | message line                | `frobulator.msg "Message" "[ detail ]"`     |
| `frobulator.add`   | add/create line             | `frobulator.add "Message" "[ detail ]"`     |
| `frobulator.rem`   | remove/delete line          | `frobulator.rem "Message" "[ detail ]"`     |
| `frobulator.ret`   | retain/keep line            | `frobulator.ret "Message" "[ detail ]"`     |
| `frobulator.rel`   | release line                | `frobulator.rel "Message" "[ detail ]"`     |
| `frobulator.fwd`   | forward/progress line       | `frobulator.fwd "Message" "[ detail ]"`     |
| `frobulator.rev`   | reverse/back line           | `frobulator.rev "Message" "[ detail ]"`     |
| `frobulator.stp`   | stop line                   | `frobulator.stp "Message" "[ detail ]"`     |
| `frobulator.dwl`   | download line               | `frobulator.dwl "Message" "[ detail ]"`     |
| `frobulator.upl`   | upload line                 | `frobulator.upl "Message" "[ detail ]"`     |
| `frobulator.lnk`   | link line                   | `frobulator.lnk "Message" "[ detail ]"`     |
| `frobulator.scs`   | success line                | `frobulator.scs "Message" "[ detail ]"`     |
| `frobulator.err`   | error line                  | `frobulator.err "Message" "[ detail ]"`     |
| `frobulator.ins`   | insert/input line           | `frobulator.ins "Message" "[ detail ]"`     |
| `frobulator.cpt`   | complete line               | `frobulator.cpt "Message" "[ detail ]"`     |
| `frobulator.url`   | url line                    | `frobulator.url "https://example.com"`      |
| `frobulator.ask`   | question prompt (no newline)| `frobulator.ask "Enter value"`              |
| `frobulator.ipt`   | input prompt (no newline)   | `frobulator.ipt "Enter value"`              |
| `frobulator.usr`   | user prompt (no newline)    | `frobulator.usr "Enter value"`              |
| `frobulator.nul`   | continue line, retain color | `frobulator.nul "continued output"`         |
| `frobulator.ind`   | continue line, clear color  | `frobulator.ind "continued output"`         |

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
[ i ] Checking dependencies ///////////////////////////////// [ curl ]
```

This pattern applies to most commands that forward their arguments through `frobulator.pmt`, including color commands, marker commands, and structured prompt helpers.

### frobulator.prompt

generic marker/color prompt engine used by status marker wrappers.

```bash
frobulator.prompt blue "${marker_inf}" true "" set "Building" "[ frobulator ]"
```

## structured prompt helpers

### frobulator.ltr

prints a lettered step marker.

```bash
frobulator.ltr "a" "Select source directory"
```

### frobulator.num

prints a numbered step marker.

```bash
frobulator.num "1" "Install dependencies"
```

### frobulator.sep

prints a separator line.

```bash
frobulator.sep
```

### frobulator.ntf

prints a framed notice block.

```bash
frobulator.ntf round inf "Notice" "The setup process is ready."
```

### frobulator.separate

prints a larger separation block for prompts or warnings.

```bash
frobulator.separate
```

### frobulator.read

captures user input after prompt helpers.

```bash
frobulator.ask "Continue?" "[ y/n ]"
frobulator.read
```

### frobulator.script

prints a script startup banner.

```bash
frobulator.script "Setting up ${script#*-}"
```

### frobulator.type

types text gradually for prompt effects.

```bash
frobulator.type 0.03 "Preparing environment..."
```

### frobulator.timeout

shows a timeout marker for a number of seconds.

```bash
frobulator.timeout 5
```

### frobulator.clear

clears prompt output during checkpointed script paging.

```bash
frobulator.clear
```

### frobulator.countdown

shows a countdown before continuing.

```bash
frobulator.countdown 10 "Starting install" "[ press ctrl+c to cancel ]"
```

## process and progress helpers

### frobulator.process

waits on a background process and prints process completion feedback.

```bash
apt-get update &
frobulator.process "Updating package index"
```

### frobulator.progress

waits on a background process and prints ongoing progress feedback.

```bash
curl -L "${url}" -o "${file}" &
frobulator.progress "Downloading" "[ ${file} ]"
```

### frobulator.bar

runs a command with an estimated progress bar.

```bash
frobulator.bar sleep 5
```

### frobulator.temporary

creates temporary directories from a string or array.

```bash
frobulator.temporary "build"
```

### frobulator.trap

registers cleanup handling for temporary runtime resources.

```bash
frobulator.temporary "build"
frobulator.trap
```

### frobulator.continue

evaluates and reports the previous command exit status.

```bash
make all
frobulator.continue
```

### frobulator.complete

runs a command and reports completion through frobulator status output.

```bash
frobulator.complete "make all"
```

## filesystem helpers

### frobulator.directory

creates directories from a path/name or array.

```bash
frobulator.directory "${HOME}/.config" "frobulator"
```

### frobulator.write

appends content to files.

```bash
frobulator.write "enabled=true" "${HOME}/.config/frobulator" "config"
```

### frobulator.flag

overwrites content to files, useful for checkpoint flags.

```bash
frobulator.flag "ready" "${temporary_directory}" "checkpoint"
```

### frobulator.file

creates files from a path/name or array.

```bash
frobulator.file "${HOME}/.config/frobulator" "config"
```

### frobulator.keep

keeps selected files and removes unselected files from a directory.

```bash
frobulator.keep "${HOME}/Downloads" "important.zip"
```

### frobulator.delete

deletes selected files from a directory.

```bash
frobulator.delete "${temporary_directory}" "old-file.tmp"
```

### frobulator.copy

copies files or directories.

```bash
frobulator.copy "${source_directory}" "${target_directory}" "config"
```

### frobulator.move

moves files or directories.

```bash
frobulator.move "${source_directory}" "${target_directory}" "archive.tar.gz"
```

### frobulator.link

creates symbolic links.

```bash
frobulator.link "${source_directory}" "${target_directory}" "config"
```

### frobulator.image

handles image-related output/operations.

```bash
frobulator.image "${image_file}"
```

## network helpers

### frobulator.http

validates HTTP status before network operations.

```bash
frobulator.http "https://example.com/file.tar.gz"
```

### frobulator.status

parses HTTP status responses for download/upload flows.

```bash
frobulator.status "https://example.com/file.tar.gz"
```

### frobulator.download

downloads files after status verification.

```bash
frobulator.download "https://example.com/file.tar.gz" "${download_directory}" "file.tar.gz"
```

### frobulator.upload

uploads data or files.

```bash
frobulator.upload POST "https://example.com/upload" "${archive_file}"
```

## command wrappers

### frobulator.silence

runs a command with all output redirected to the sink.

```bash
frobulator.silence "apt-get update"
```

### frobulator.log

runs a command and redirects output to a timestamped log.

```bash
frobulator.log "apt-get install curl"
```

## user input helpers

### frobulator.password

captures masked password input.

```bash
frobulator.password
```

### frobulator.input

captures and validates user input against an array.

```bash
options=( "one" "two" "three" )
frobulator.input options
```

### frobulator.dialog

generates desktop dialogs through supported desktop helpers.

```bash
frobulator.dialog "info" "Setup complete"
```

## package helpers

### frobulator.clean

runs package-manager cleanup.

```bash
frobulator.clean
```

### frobulator.hold

marks packages as held/frozen.

```bash
frobulator.hold "firefox-esr"
```

### frobulator.release

removes package hold/freeze state.

```bash
frobulator.release "firefox-esr"
```

### frobulator.failsafe

updates package sources before installing to avoid missing package errors.

```bash
frobulator.failsafe "curl"
```

### frobulator.install

installs packages.

```bash
frobulator.install "curl"
```

### frobulator.require

checks and installs required packages.

```bash
frobulator.require "curl"
```

### frobulator.reinstall

reinstalls packages.

```bash
frobulator.reinstall "curl"
```

### frobulator.update

updates package lists.

```bash
frobulator.update
```

### frobulator.upgrade

upgrades installed packages.

```bash
frobulator.upgrade
```

### frobulator.purge

purges packages.

```bash
frobulator.purge "unused-package"
```

## process, privilege, and result helpers

### frobulator.terminate

forcefully terminates processes.

```bash
frobulator.terminate "rogue-process"
```

### frobulator.exit

exits a process/program instance with frobulator messaging.

```bash
frobulator.exit "setup"
```

### frobulator.result

reports checkpoint result status.

```bash
frobulator.result "${checkpoint_file}"
```

### frobulator.user

prints the active shell user.

```bash
frobulator.user
```

### frobulator.assess

assesses privileges and requirements.

```bash
requirements=( curl git )
frobulator.assess requirements
```

### frobulator.escalate

relaunches the current script as superuser.

```bash
export self_arguments="${@}"
frobulator.escalate
```

## archive helpers

### frobulator.archive

creates archives for backup or packaging.

```bash
frobulator.archive "backup.tar.gz" "gz" "${HOME}/Documents"
```

### frobulator.extract

extracts known archive types.

```bash
frobulator.extract "backup.tar.gz" "${target_directory}"
```

## complete command index

```text
frobulator.plo
frobulator.pmt
frobulator.color
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
frobulator.prompt
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
frobulator.separate
frobulator.read
frobulator.script
frobulator.type
frobulator.timeout
frobulator.clear
frobulator.countdown
frobulator.process
frobulator.progress
frobulator.bar
frobulator.temporary
frobulator.trap
frobulator.continue
frobulator.complete
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
frobulator.terminate
frobulator.exit
frobulator.result
frobulator.user
frobulator.assess
frobulator.escalate
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

[[ Frobulator // Project Page ]](https://github.com/nathaneltitane/frobulator) [ Version // 05-31-2026 ]

### Enjoying Frobulator? Buy me a coffee to show your appreciation!

[![Donate](https://img.shields.io/badge/Paypal-2f343f.svg?style=for-the-badge&logo=paypal&label=Donate)](https://www.paypal.com/donate?hosted_button_id=ZW3CDCANHJCWJ)
