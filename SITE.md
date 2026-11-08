# Site

Every section of [frobulator.app](https://frobulator.app). `tools/frobulator-site` builds the page from this file. Each `##` section starts with a comment that sets its page id, icon and heading, and its first paragraph is the lead. `- **Title.** text` lines become points and `1. **Title.** text` lines steps, a `<!-- table class -->` comment sets the class of the table below it, and a `<!-- gallery class -->` comment turns the images and video links below it into one gallery, the title of a single image setting its figure class. A `<!-- video -->` comment turns the link below it into a looping video. A `<!-- include name -->` comment places content read from the scripts, a paragraph made of links ending in › becomes a more link, and `@NAME@` placeholders are filled with counts read from the scripts.

## Overview

<!-- overview | orange | OVERVIEW // A FUNCTION LIBRARY FOR SHELL SCRIPTS -->

Frobulator is a shell parser and scripting function library. Source it at the top of a Bash script and every message, prompt, download, package install and checkpoint gets the same clean, colorized, 80 column output - all while making redundant and complex code bits a thing of the past.

<!-- include demonstration -->

- **Line markers.** Information, warning, success, error, download and two dozen more message types, each with its own glyph and color.
- **Standard lines.** Every line is parsed to the terminal width, never below 80 columns: long text folds into paragraphs, long details shorten with an ellipsis.
- **Prompts and input.** Question, input, user and return prompts, lettered and numbered steps, masked password input and labelled value capture.
- **Progress feedback.** Tickers, activity bars, countdowns and typed text, with background jobs that never print shell job notices.
- **Checkpoints.** Status tracking that halts, fails or carries on, logs failures and skips completed steps on the next run.
- **Files and network.** Directory, write, copy, move, link and delete helpers with ownership kept, plus downloads and uploads reported by HTTP status.
- **Packages.** One set of install, require, hold, purge and update commands for apt on Debian and Ubuntu, and dnf on Fedora.
- **Privileges.** Escalation through sudo that keeps the original arguments, systemd service control and readable permission scopes.
- **Archives.** Create and extract tar, gzip, bzip2, zip, 7z and rar archives, with the needed tool installed on demand.
- **Self updating.** The bootstrap header fetches the latest release and only replaces the local copy with a newer one.

### Scripts only

The current set of assertions upon which Frobulator is built restricts its functionality to scripts exclusively, at least for the time being.

## Install

<!-- install | amber | INSTALL // ONE FILE IN ~/.LOCAL/BIN -->

Frobulator is a single Bash file. Run it to install it to `${HOME}/.local/bin`, then source it from your scripts - or let the bootstrap header do both.

### Install

```
curl -s -L get.frbltr.app > frobulator && bash frobulator
```

Running the script directly installs it - no arguments default to `--install`. Sourcing is unaffected: the options only apply when the file is executed.

```
./frobulator              # install (default)
./frobulator --install    # install
./frobulator --uninstall  # uninstall - scripts using frobulator fetch it again on their next run
./frobulator --help       # usage
```

### Source

```
. "${HOME}"/.local/bin/frobulator
```

### Version

<p>The release is identified by the literal <code>update="YYYY-MM-DD-HHMM"</code> line at the top of the file. The bootstrap reads it as text (<code>grep '^update='</code>) and downloads a new copy when the online value sorts after the installed one - bump it, with a four digit time, for every release, including same day releases. <code>version</code> is derived from it.</p>

[View the library source ›](https://github.com/nathaneltitane/frobulator/blob/main/frobulator)

## Bootstrap

<!-- bootstrap | yellow | BOOTSTRAP // THE STANDARD SCRIPT HEADER -->

Start every script with this header. It fetches the latest Frobulator with curl or wget, sources it once, escalates when needed and prints the script banner. The sections below it are the standard layout of a Frobulator script.

1. **Arguments.** `self_arguments` keeps the original arguments for `frobulator.escalate` - spaces, quotes and empty arguments intact.
2. **Sudo aware.** Run as root through sudo, the header keeps using the invoking user's copy in their home directory.
3. **Newer only.** The download from `get.frbltr.app` replaces the local copy only when its `update=` line sorts after it.
4. **Loaded once.** `frobulator_loaded` skips the download and sourcing when Frobulator is already loaded.
5. **Requirements.** The `list` array feeds `frobulator.require`, which installs whatever command or package is missing.

<!-- include bootstrap -->

[Get the bootstrap header ›](https://github.com/nathaneltitane/frobulator/blob/main/bootstrap)

## Markers

<!-- markers | green | MARKERS // @MARKERS@ LINE TYPES -->

Every marker prints a message, an optional detail and an optional fill character on one line as wide as the terminal, followed by a blank line from `frobulator.brk` - scripts do not add `echo` after messages. Prompts stay on the same line for the answer.

<!-- include markers -->

- **Carry-over lines.** `frobulator.nul` keeps the color of the line above, `frobulator.ind` clears it - both rejoin the line above so details stay together.
- **Steps.** `frobulator.ltr` and `frobulator.num` print lettered and numbered steps, optionally in a palette color.
- **Separators.** `frobulator.sep` prints a full width line, uncolored unless a palette color is given.
- **Notices.** `frobulator.ntf` frames a title and message in square, round, heavy, double, dots, matrix, tech, skel or ascii style.

### Arguments

Markers accept direct strings, variables, arrays, command substitutions or values generated elsewhere in the script. All of them are normalized through `frobulator.pmt`, so alignment, wrapping and span generation stay consistent.

```
frobulator.inf "Checking dependencies" "[ curl ]"

prompt_arguments=(
	"Checking dependencies"
	"[ curl ]"
)

frobulator.inf "${prompt_arguments[@]}"
```

<!-- include example -->

## Colors

<!-- colors | emerald | COLORS // @COLORS@ NAMED 256 COLOR CODES -->

Every palette color has its own line command, and its name is accepted wherever a color is: numbered steps, separators and the critter. Colors are set only when output is a terminal - through `tput`, or ANSI sequences without it - and stay empty otherwise.

<!-- include swatches -->

### Add your own

`frobulator.palette` adds a named color at run time from a 256 color code. Adding an existing name updates its color. The colors shown here follow a dark terminal theme, with the darkest codes lifted for reading: your terminal decides how they look.

```
frobulator.palette cornflower 68
frobulator.num cornflower 1 "Soft blue line" "[ value ]"
```

## Checkpoints

<!-- checkpoints | teal | CHECKPOINTS // STATUS, LOGS AND RESUMES -->

Frobulator replaces the `if [ "${?}" -ne 0 ]` blocks of a script with one line checks. Failures are recorded, logged to `~/.local/var/log/<script>-<stamp>.log` and summed up at the end.

<!-- table wrap -->
| Command | On failure |
| --- | --- |
| `frobulator.complete` | Reports `Incomplete - &lt;label&gt;`, logs the failure and carries on |
| `frobulator.complete halt` | Logs, prints the summary and exits the script - also inside a function |
| `frobulator.halt` | Prints the message and exits with the failed command's own status |
| `frobulator.fail` | Returns the failed status inside a function - follow it with `&#124;&#124; return` |
| `frobulator.track` | Records the failure in the calling function's `status` |
| `frobulator.result` | Reports overall success or the failure count, then clears the records |

```
zipalign -p -f 4 "${file_unsigned}" "${file_aligned}"
frobulator.complete halt "Align failed" "[ ${file_aligned} ]"

rm -f "${cache}"
frobulator.complete

frobulator.complete "${checkpoint_directory}" "make-all" make all

frobulator.result "${script}"
```

The checkpoint form runs a command once: it is skipped while `"${path}/${checkpoint}"` exists, and the checkpoint file is only created on success. Run the script again after a failure and it resumes where it stopped.

## Packages

<!-- packages | cyan | PACKAGES // APT AND DNF -->

The package manager is picked from the distribution family in `/etc/os-release`: `debian` and its derivatives use apt, `fedora` and its derivatives use dnf, or yum when dnf is missing. The family decides, not the commands on the `PATH`, so a Fedora container under Termux uses dnf even with Termux's apt in reach.

| Command | apt | dnf |
| --- | --- | --- |
| `frobulator.update` | apt-get update | dnf makecache |
| `frobulator.upgrade` | apt-get dist-upgrade | dnf upgrade |
| `frobulator.install` | apt search, then install | dnf install |
| `frobulator.require` | apt-file search | dnf repoquery --whatprovides |
| `frobulator.reinstall` | apt-get install --reinstall | dnf reinstall |
| `frobulator.purge` | apt-get purge --autoremove | dnf remove |
| `frobulator.hold` | apt-mark hold | dnf versionlock add |
| `frobulator.release` | apt-mark unhold | dnf versionlock delete |
| `frobulator.clean` | autoremove, autoclean, clean | dnf autoremove, dnf clean all |

`frobulator.require` checks commands and installed packages first and only touches the package manager when something is missing, so requirements that are already met need no root access. Package name globs such as `"libssl*"` install every match.

## Permissions

<!-- permissions | sky | PERMISSIONS // SCOPES INSTEAD OF MODES -->

`frobulator.permissions` sets the exact mode from a scope and optional modifiers, in place of raw `chmod` numbers. Directories always keep execute for whoever can read them.

- **public.** You edit, everyone reads.
- **group.** You edit, your group reads, no access for others.
- **private.** Only you.
- **execute / readonly.** Whoever can read can also run / nobody edits, including you.

<!-- include permissions -->

```
frobulator.permissions public execute "${path}/${file}"
frobulator.permissions private readonly "${HOME}/.ssh/id_ed25519"
```

## Functions

<!-- functions | blue | FUNCTIONS // EVERY COMMAND AND EXAMPLE -->

All @FUNCTIONS@ Frobulator functions with their arguments and examples. Search them, or pick a group.

<!-- include functions -->

