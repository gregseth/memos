Shell Fu
========

Useful commands
---------------

### *nix

    script <filename>

Saves everything output in the console in the file, until EOT (^D).

    cd -

Go to previous directory (not the parent one like cd ..).

    C-l

The same as `clear`: cleans the console.

    tac

The same as `cat` but in reverse order (from the las line of the file).

    dialog

Creates a GUI like environment. See man page.

    nc -l PORT

Listens to the PORT, and outputs everything sent to it to stdout.

    nc -l PORT < index.htm

Creates a mini webserver, serving only index.htm

    nc HOST PORT

Send the stdin to the HOST:PORT.

    ^err^new

Replaces 'err' by 'new' in the previous entry.

    pgrep <processname>

PID of the processus named processname.

    C-r + start typing

Search the history

    watch -d <command>

Executes command every 2sec (can be changed with --interval=secs) and show
the differences in the output.

    reset

Cleans the term, when binary output messed the thing up.

    lsof

List open files (and by which process).

    tr "[:upper:]" "[:lower:]"

Convert uppsercase character to lowercase.

    iconv

Convert charsets

    | wc -m

String length, see manpage for line or byte count.

    expr length <string>

Gives the length of the string. See: ${#var}.

    echo "ibase=xx; obase=yy; Z" | bc

Convert Z from base xx to base yy.

    mkdir -p path/to/{dir1,dir2,.../{dir1,Dir2}}

Creates the whole tree.

    {n..m}

Is equivalent to $(seq n m), can be zero-padded. eg: {001..100}

    rename "s/<regex>/<repl>/" <files>

Batch rename files with regexes.

    fmt -<n> -s <file>

Reformats the content of <file> to <n> columns width.

    true, :, false

Does noting, and respectively returns 0, 0 and 1.

    ssh -L local-port:address:port ssh-server

Redirects local-port on localhost to port on address.

    ssh -R remote-port:address:port ssh-server

Redirects remote-port on ssh-server to port on address.

    echo <dir1> ... <dirN> | xargs -n 1 cp <file>

Copies file to each directory echoed.

    \command

Run command, unaliased

    gnome-desktop-item-edit --create-new <dir>

Opens dialog to create a new desktop shortcut. Here's a template for
manually creating a .desktop file (must have +x permissions):

    #!/usr/bin/env xdg-open
    [Desktop Entry]
    Version=X.Y.Z
    Name=MyApp
    GenericName=TextEditor
    Type=Application
    Icon=/path/to/icon.png
    Exec=/path/to/exe
    Path=/path/to/execution/dir
    Terminal=false


### OS X

    sudo mdfind "com_apple_backup_excludeItem = 'com.apple.backupd'"

Print a list of the files ignored by Time Machine.

    nettop

View the live netowrk traffic. Use 'c' to view collapse processes, 'd' to
see the delta and not the cumulative values and 'p' to show human readable
values. 'q' to quit.

    sudo nvram SystemAudioVolume=%XX
    sudo nvram -d SystemAudioVolume

Sets the volume of the BANG on startup to XX%, resp. resets it to its
default value.

    defaults write com.apple.notificationcenterui bannerTime SECONDS
    defaults delete com.apple.notificationcenterui bannerTime

Sets the delay to SECONDS for notifications, resp. resets to default time.

    chflags [no]hidden FILE

(Un)sets the hidden flag on FILE. The file is invisible in the Finder but
still present and accessble through its name. Hiding a file may have
side-effects such as preventing overwriting.

    sqlite3 "~/Library/Application Support/Dock/A04B2AC4-BFE4-421F-9C1A-EB823C63684A.db" \
    "DELETE from apps; DELETE from groups WHERE title<>''; DELETE from items WHERE rowid>2;" \
    && killall Dock

Cleans the Launchpad database.

    xcodebuild -list -project <PROJECT.xcodeproj>
    xcodebuild -scheme <SCHEME> [build]

Lists avialable schemes, then compile the project.

    mdls -name kMDItemVersion /path/to/Application.app

Return the version number for that application.

    security -v unlock-keychain ~/Library/Keychains/login.keychain

Unlocks the keychain from command line.


### Windows

    mklink [/D] LNK TARGET

Creates a symlink of TARGET to LNK. Use /D for directories.

    icacls * /reset /t

Restore authorisations of all files and directories (recursively) in the
current directory.

    echo | set /p=TEXT

Prints TEXT without the trailing [CR][LF].

    msizap T {PRODUCT_CODE}

Force deletion of the product from the MSI database. See help for more options.

    control userpasswords2

Uncheck the box "Users must enter a password to open a session" in order to
enable auto-login.

    Restart-Computer / Stop-Computer

Powershell commands (quite self-explanatory).

    corflags ASSEMBLY

Tells if a .NET assembly is 32 or 64 bits. The results are to be interpreted
with the following chart:

 Option    | PE    | 32BIT
:----------|:------|:--------
 x86       | PE32  | 1
 Any CPU   | PE32  | 0
 x64       | PE32+ | 0


SETUID, SETGID & sticky bit
---------------------------

### On executatble files

  - SETUID: Allows any user to run the file as the owner.
  - SETGID: Allows any user to run the file as the owner's group.
  - Sticky bit: Makes the program stay in memory after execution. -- Not really
      used anymore.

### On directories

  - SETUID: N/A
  - SETGID: Any file created inside the directory belong to the group of
      the directory.
  - Sticky bit: Only the owner of a file in that directory can delete it.

### Chmod

In octal notation, SETUID=4, SETGID=2 and sticky=1. They prefix the classic
notation: rwsr-sr-x gives 6755.
If the bit are set, but there's no execution rights, they appear as capitalised:
rwSr-Sr-T (7644).


Scripting
---------

### Know where you are

In a script, get the full path of the script. The BASH_SOURCE is used insteal of
$0 to work both when script is executed or sourced.

    PATH=$(cd `dirname "${BASH_SOURCE[0]}"` && pwd)
    FILE=$(`basename "${BASH_SOURCE[0]}")


### Heredoc

The HEREDOC syntax allows to inline the content of a file in a command line
or a script. The syntax is the following:

    <some command> << END_TEXT
    <multiline unescaped text goes here>
    END_TEXT

END_TEXT is used as the delimiting identifier the "file" goes on until the
identifier is found by itself on a separate line. Appending a minus sign to the
<< has the effect that leading tabs (not spaces) are ignored. By default,
variables are interpolated and commands in backticks are evaluated. This can be
disabled by quoting the identifier.

Windows batch
-------------

 Batch        | Unix eq.   | Description
:-------------|:-----------|:----------
 `%n`         | `$n`       | The n'th parametr of the function (0-indexed).
 `set VAR=`   | `VAR=`     | Assing variable.
 `%VAR%`      | `$VAR`     | Value of the variable.
 `%VAR:~x,y%` |            | Substring of length y from the x-th character
 `%VAR:x=y%`  |            | Replace all occurences of x by y in VAR

    FOR /L %%i IN (start,step,stop) DO ( ECHO %%i )
    for i in {start,stop} ; do; echo $i; done

Iterates i from start to stop with step or 1 increment.
Notice that in a batch the % sign before the variable name must be doubled (to
avoid confusion with paramters, sic!) and that the variable name must be a 
single letter.


Special vars
------------

 Variable      |Description
:--------------|:----------
 `!#`          |Current command line (can be combined with others, eg: `!#$`).
 `!!`          |Last command line.
 `!$`          |Last argument of last command line.
 `!^`          |First argument of last command line.
 `!*`          |All arguments of last command line.
 `!:n`         |n-th word of last command line (n=0 is the command name).
 `!:n-m`       |n-th to m-th word of last command line.
 `!:^`         |First word of last command line.
 `!:$`         |Last word of last command line.
 `!cmd`        |Runs the most recent command starting with cmd.
 `!cmd:p`      |Prints the most recent command starting with cmd.
 `!cmd:s/m/r/` |Runs the most recent command starting with cmd replacing m by r.
 `!?match`     |Runs the most recent command that contains match.
 `$n`          |The n-th parameter (0 is the command, 1 is the 1st parameter).
 `$*`          |All the parameters in one string (`"$1 $2 … $n"`).
 `$@`          |All the parameters in n strings (`"$1" "$2" … "$n"`).
 `$#`          |The number of parameters of the command.
 `$?`          |The return code of the last comand.
 `$-`          |The parameters used at shell launch.
 `$$`          |PID of the shell in which the command is executed.
 `$!`          |PID of the last background command (started with & or C-Z ; bg).
 `$_`          |Last command or parameter typed.
 `${#var}`     |Length of `$var`
 `${=var}`     |Splits `$var` into words.
 `${var[:]-word}`       |Returns `$var` if var is set [non null] else word.
 `${var[:]+word}`       |Returns word if var is set [non null] else nothing.
 `${var#[#]match}`      |Deletes the shortest [longest] matching part (beg).
 `${var%[%]match}`      |Deletes the shortest [longest] matching part (end).
 `${var/[/]match/repl}` |Replaces the first [all the] matching parts by repl.
 `$'\t'`       |Litteral escaped character

ZSH
---

 Expression                | Description
:--------------------------|:-----------
 `${(l:n::f::e:)var}`      | Prints `$var`, left padded on n-width with f. The
                           | padding is optionnally ended by `e.
 `${(r:n::f::e:)var}`      | Prints `$var`, right padded on n-width with f. The
                           | padding is optionnally ended by e.
 `$var:[g]s/match/repl[/]` | Replaces match by repl in `$var`
 `$var:t`                  | If `$var` is a path, ouputs the dirpath.
 `$var:h`                  | If `$var` is a path, ouputs the filename.
 `$var:l`                  | To lowercase
 `$var:u`                  | To uppercase
 `${+var}`                 | Returns 1 if `$var` is set else 0.


Terminfo alternate character set (ACS)
--------------------------------------

Use `$terminfo[smacs]` to enable the ACS, and `$terminfo[rmacs]` to disable it.
The characters are available in `$terminfo[acsc]` as an inline list of key/value
pairs the key being the ASCII char, and the value the corresponding char
to escape. It can be retreived as a ZSH associative array (named ACS here) 
as follows:

    typeset -A ACS
    set -A ACS ${(s..)terminfo[acsc]}

  The following snippet shows how to print the equivalency table (depends on
  your term and and the font you're using):

    for l in $ACS
    do
      echo "$l ($ACS[$l]) : $terminfo[smacs]${ACS[$l]:--}$terminfo[rmacs]"
    done

    w (w) : ┬
    f (f) : °
    x (x) : │
    g (g) : ±
    y (y) : ≤
    z (z) : ≥
    i (i) : ␋
    { ({) : π
    j (j) : ┘
    | (|) : ≠
    k (k) : ┐
    } (}) : £
    l (l) : ┌
    ~ (~) : ·
    m (m) : └
    n (n) : ┼
    o (o) : ⎺
    p (p) : ⎻
    q (q) : ─
    ` (`) : ◆
    r (r) : ⎼
    a (a) : ▒
    s (s) : ⎽
    t (t) : ├
    u (u) : ┤
    v (v) : ┴


ANSI special chars
------------------

### Font formatting

These can be set by echoing `\e[#m<custom_text>\e[0m`

   - `\e` is the ANSI escape code (usually `\e == \033`)
   - `#` is in the form `n;m` with `n` the code for the foreground color, and 
       `m` the code for the background color:

 Style      | Text
:-----------|:----
 none       | 0
 bold       | 1
 italic     | 2
 underline  | 4
 inverted   | 7
 invisible  | 8
 strike     | 9

 Color        |  Text | Background
:-------------|:------|:----------
 black        |  30   | 40
 red          |  31   | 41
 green        |  32   | 42
 yellow       |  33   | 43
 blue         |  34   | 44
 purple       |  35   | 45
 teal         |  36   | 46
 light gray   |  37   | 47
 dark gray    |  90   | 100
 light red    |  91   | 101
 light green  |  92   | 102
 ligth yellow |  93   | 103
 light blue   |  94   | 104
 pink         |  95   | 105
 turquoize    |  96   | 106
 white        |  97   | 107

### ANSI cursor positioning

 Code      | Name | Effect
:----------|:-----|:------
 `\e[nA`   | CUU  | Moves cursor up n lines
 `\e[nB`   | CUD  | Moves cursor down n lines
 `\e[nC`   | CUF  | Moves cursor right n lines
 `\e[nD`   | CUB  | Moves cursor left n lines
 `\e[nE`   | CNL  | Moves cursor to the beginning of the line, n lines down
 `\e[nF`   | CPL  | Moves cursor to the beginning of the line, n lines up
 `\e[nG`   | CHA  | Moves the cursor to column n
 `\e[n;mH` | CUP  | Moves the cursor to row n and column m (1 based values)
 `\e[n;mf` | HVP  | Moves the cursor to row n and column m (1 based values)
 `\e[nJ`   | ED   | Clears part of the screen:
 .         | .    |  n=0: from cursor to end of screen
 .         | .    |  n=1: from beginning of the screen to cursor
 .         | .    |  n=2: the entire screen
 `\e[nK`   | EL   | Clears part of the line:
 .         | .    |  n=0: from cursor to end of line
 .         | .    |  n=1: from beginning of the line to cursor
 .         | .    |  n=2: the entire line
 `\e[nS`   | SU   | Scrolls the page by n lines up
 `\e[nT`   | SD   | Scrolls the page by n lines down 
 `\e[s`    | SCP  | Saves cursor position
 `\e[u`    | RCP  | Restores cursor position


Manpages number
---------------

 Section  | Topic
---------:|:----------------------------------------------------
 1        | Commands available to users.
 2        | Unix and C system calls.
 3        | C library routines for C programs.
 4        | Special file names.
 5        | File formats and conventions for files used by Unix.
 6        | Games.
 7        | Word processing packages.
 8        | System administration commands and procedures.

Shortcuts
---------

 Command | Description
:--------|:------------------------------------------------------------------
 C-v     | Prints the litteral command character. (eg: C-V C-M outputs ^M).
 C-r     | Shell reverse history search.
 C-a     | Go to beginning of line.
 C-e     | Go to end of line.
 C-u     | Cuts the line from the beginning to the cursor.
 C-k     | Cuts the line from the cursor to EOL.
 C-l     | Cuts the screen.
 C-y     | Pastes the previously cut text.
 C-c     | Terminate the command.
 C-d     | End of transmission.
 C-z     | Suspends the current command in background, restart with fg or bg.


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxNjk5MDc4NiwxNTA3OTk4MjUwLC0xMz
Q3NTk5MzE0XX0=
-->