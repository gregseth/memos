# Bash
## Input inline data
### `<<` heredoc
Usage:
```bash
cat <<DELIMITER
the 
multiline
document
read as input
DELIMITER
```
Two options are available:
- if the DELIMITER is single or double quoted, no variable expansion or command substitution is performed
- the operator `<<-` strips the leading `<TAB>` characters (not `<SPACE>`) in the heredoc
The output of the can be piped (`|`) or redirected (`>`) on the line of th command, after `<<[-]DELIMITER`. 
### `<<<` herestring
The following two lines are equivalent
```bash
echo 'string' | cat
cat <<< 'string'
```
## `<()` process substitution
Redirects the output of a list of commands to a pseudo file. the input of another.
The following two lines are equivalent :
```bash
{ echo 1; echo 2; echo 3; } | wc
wc < <(echo 1; echo 2; echo 3)
```
### Working with variables
assuming there’s a defined variable `$v`, the following do the same:
```bash
echo "$v" | wc
wc < <(echo "$v")
wc <<< "$v"
cat <<EOF | wc
$v
EOF
```
### Examples
#### grepping variables
```bash
grep "needle" <<< "$haystack"
```
#### inline diff
```bash
diff -y <(cmd1) <(cmd2)
```
# Variables
## Special variables
| Variable      | Description                                                      |
| :------------ | :--------------------------------------------------------------- |
| `!#`          | Current command line (can be combined with others, eg: `!#$`).   |
| `!!`          | Last command line.                                               |
| `!$`          | Last argument of last command line.                              |
| `!^`          | First argument of last command line.                             |
| `!*`          | All arguments of last command line.                              |
| `!:n`         | n-th word of last command line (n=0 is the command name).        |
| `!:n-m`       | n-th to m-th word of last command line.                          |
| `!:^`         | First word of last command line.                                 |
| `!:$`         | Last word of last command line.                                  |
| `!cmd`        | Runs the most recent command starting with cmd.                  |
| `!cmd:p`      | Prints the most recent command starting with cmd.                |
| `!cmd:s/m/r/` | Runs the most recent command starting with cmd replacing m by r. |
| `!?match`     | Runs the most recent command that contains match.                |
| `$n`          | The n-th parameter (0 is the command, 1 is the 1st parameter).   |
| `$*`          | All the parameters in one string (`"$1 $2 … $n"`).               |
| `$@`          | All the parameters in n strings (`"$1" "$2" … "$n"`).            |
| `$#`          | The number of parameters of the command.                         |
| `$?`          | The return code of the last comand.                              |
| `$-`          | The parameters used at shell launch.                             |
| `$$`          | PID of the shell in which the command is executed.               |
| `$!`          | PID of the last background command (started with & or C-Z ; bg). |
| `$_`          | Last command or parameter typed.                                 |
## Variable manipulation
| Variable               | Description                                                      |
| :--------------------- | :--------------------------------------------------------------- |
| `${#var}`              | Length of `$var`                                                 |
| `${=var}`              | Splits `$var` into words.                                        |
| `${var[:]-word}`       | Returns `$var` if var is set [non null] else word.               |
| `${var[:]+word}`       | Returns word if var is set [non null] else nothing.              |
| `${var#[#]match}`      | Deletes the shortest [longest] matching part (beg).              |
| `${var%[%]match}`      | Deletes the shortest [longest] matching part (end).              |
| `${var/[/]match/repl}` | Replaces the first [all the] matching parts by repl.             |
| `$'\t'`                | Litteral escaped character                                       |
