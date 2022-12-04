# MIT-Missing-Semester

# Topic 1: The Shell

# Shell basic

When you launch your terminal, you will see a prompt that often looks a little like this:

```missing:~$ ```

This is the main textual interface to the shell. It tells you that you are on the machine missing and that your “current working directory”, or where you currently are, is `~` (short for `“home”`). The `$` tells you that you are not the root user (more on that later). At this prompt you can type a command, which will then be interpreted by the shell. The most basic command is to execute a program:

`cd -`

`pwd, echo, which, cd, ls, mv, cp, rm, rmdir, mkdir, man`

`.` refers to the current directory and `..` refers to its parent directory

`rm -r` 

`ls -l`

This gives us a bunch more information about each file or directory present. First, the `d` at the beginning of the line tells us that missing is a directory. Then follow three groups of three characters (`rwx`). These indicate what permissions the owner of the file (missing), the owning group (users), and everyone else respectively have on the relevant item. A `-` indicates that the given principal does not have the given permission. Above, only the owner is allowed to modify (`w`) the missing directory (i.e., add/remove files in it). To enter a directory, a user must have “search” (represented by “execute”: `x`) permissions on that directory (and its parents). To list its contents, a user must have read (`r`) permissions on that directory. For files, the permissions are as you would expect. Notice that nearly all the files in `/bin` have the `x` permission set for the last group, “everyone else”, so that anyone can execute those programs.

`control L` clears terminal.

# Connecting programs

In the shell, programs have two primary “streams” associated with them: their input stream and their output stream. When the program tries to read input, it reads from the input stream, and when it prints something, it prints to its output stream. Normally, a program’s input and output are both your terminal. That is, your keyboard as input and your screen as output. However, we can also rewire those streams!

The simplest form of redirection is `< file` and `> file`. These let you rewire the input and output streams of a program to a file respectively:

```missing:~$ cat < hello.txt > hello2.txt```

You can also use `>>` to append to a file. Where this kind of input/output redirection really shines is in the use of pipes. The `|` operator lets you “chain” programs such that the output of one is the input of another:

```
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

# sudo

`sudo su`

One thing you need to be root in order to do is writing to the `sysfs` file system mounted under `/sys`. `sysfs` exposes a number of kernel parameters as files, so that you can easily reconfigure the kernel on the fly without specialized tools. **Note that sysfs does not exist on Windows or macOS.**

`tee`

```
$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
/sys/class/backlight/thinkpad_screen/brightness
$ cd /sys/class/backlight/thinkpad_screen
$ sudo echo 3 > brightness
An error occurred while redirecting file 'brightness'
open: Permission denied
```

This error may come as a surprise. After all, we ran the command with `sudo`! This is an important thing to know about the shell. Operations like `|, >, and <` are done by the shell, not by the individual program. echo and friends do not “know” about `|`. They just read from their input and write to their output, whatever it may be. In the case above, the shell (which is authenticated just as your user) tries to open the brightness file for writing, before setting that as `sudo echo`’s output, but is prevented from doing so since the shell does not run as root. Using this knowledge, we can work around this:

`$ echo 3 | sudo tee brightness`

https://en.wikipedia.org/wiki/Shebang_(Unix)
**touch, chmod, cut, curl**


# Topic 2: Shell Tools and Scripting

o assign variables in bash, use the syntax `foo=bar` and access the value of the variable with $foo. Note that `foo = bar` will not work since it is interpreted as calling the foo program with arguments = and bar. In general, in shell scripts the space character will perform argument splitting. This behavior can be confusing to use at first, so always check for that.

Strings in bash can be defined with `'` and `"` delimiters, but they are not equivalent. Strings delimited with `'` are literal strings and will not substitute variable values whereas " delimited strings will.
```
foo=bar
echo "$foo"
# prints bar
echo '$foo'
# prints $foo
```
*source*

As with most programming languages, bash supports control flow techniques including `if`, `case`, `while` and `for`. Similarly, bash has functions that take arguments and can operate with them. Here is an example of a function that creates a directory and cds into it.
```
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```
Here `$1` is the first argument to the script/function. Unlike other scripting languages, bash uses a variety of special variables to refer to arguments, error codes, and other relevant variables. Below is a list of some of them. A more comprehensive list can be found here.


Commands will often return output using `STDOUT`, errors through `STDERR`, and a Return Code to report errors in a more script-friendly manner. The return code or exit status is the way scripts/commands have to communicate how execution went. A value of `0` usually means everything went OK; anything different from `0` means an error occurred.

Exit codes can be used to conditionally execute commands using `&&` (and operator) and `||` (or operator), both of which are short-circuiting operators. Commands can also be separated within the same line using a semicolon `;`.


Another common pattern is wanting to get the output of a command as a variable. This can be done with command substitution. Whenever you place `$( CMD )` it will execute `CMD`, get the output of the command and substitute it in place. For example, if you do for file in `$(ls)`, the shell will first call ls and then iterate over those values. A lesser known similar feature is process substitution, `<( CMD )` will execute `CMD` and place the output in a temporary file and **substitute the `<()` with that file’s name**. This is useful when commands expect values to be passed by file instead of by `STDIN`. For example, `diff <(ls foo) <(ls bar)` will show differences between files in dirs foo and bar.


`$0` - Name of the script
`$1` to `$9` - Arguments to the script. $1 is the first argument and so on.
`$@` - All the arguments
`$#` - Number of arguments
`$?` - Return code of the previous command
`$$` - Process identification number (PID) for the current script
`!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
`$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by . or `Alt+`.

#shell globbing

Wildcards - Whenever you want to perform some sort of wildcard matching, you can use `?` and `*` to match one or any amount of characters respectively. For instance, given files `foo`, `foo1`, `foo2`, `foo10` and `bar`, the command `rm foo?` will delete `foo1` and `foo2` whereas `rm foo*` will delete all but bar.
Curly braces `{}` - Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically. This comes in very handy when moving or converting files.

```
convert image.{png,jpg}
# Will expand to
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# Will expand to
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# Globbing techniques can also be combined
mv *{.py,.sh} folder
# Will move all *.py and *.sh files


mkdir foo bar
# This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
touch {foo,bar}/{a..h}
touch foo/x bar/y
# Show differences between files in foo and bar
diff <(ls foo) <(ls bar)
# Outputs
# < x
# ---
# > y
```


*22 min*

```#!/usr/bin/env python```

`shellcheck`

`tldr`

`ripgrep`








