# MIT-Missing-Semester

# Topic 1: The Shell

# Shell basic

When you launch your terminal, you will see a prompt that often looks a little like this:

```missing:~$ ```

This is the main textual interface to the shell. It tells you that you are on the machine missing and that your “current working directory”, or where you currently are, is `~` (short for `“home”`). The `$` tells you that you are not the root user (more on that later). At this prompt you can type a command, which will then be interpreted by the shell. The most basic command is to execute a program:

`cd -`

pwd, echo, which, cd, ls, mv, cp, rm, rmdir, mkdir, man

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



