`>>` is used for appending and `>` is used for overwriting.

A File Descriptor (FD) is a number that _describes_ a communication channel in Linux.
- FD 0: Standard Input
- FD 1: Standard Output
- FD 2: Standard Error
When you redirect process communication, you do it by FD number, though 
some FD numbers are implicit. For example, a `>` without a number
implies `1>`, which redirects FD 1 (Standard Output).
That will redirect standard error (FD 2) to the `errors.log` file.
Furthermore, you can redirect multiple file descriptors at the same 
time! For example:
```console
hacker@dojo:~$ some_command > output.log 2> errors.log
```
That command will redirect output to `output.log` and errors to 
`errors.log`.

The shell has a `>&` operator, which redirects a file descriptor _to 
another file descriptor_. This means that we can have a two-step process 
to grep through errors: first, we redirect standard error to standard 
output (`2>& 1`) and then pipe the now-combined stderr and stdout as 
normal (`|`)!

The `tee` command reads from the standard input and writes to both 
standard output and one or more files at the same time.

2nd last sol : ```/challenge/hack | tee >(/challenge/the/) >(/challenge/planet)```
This is done using what's called [Process Substitution](https://www.gnu.org/software/bash/manual/html_node/Process-Substitution.html). If you write an argument of `>(rev)`, bash will run the `rev` command (this command reads data from standard input, reverses its order, and writes it to standard output!), but hook up its input to a temporary file that it will create. This isn't a _real_ file, of course, it's what's called a _named pipe_, in that it has a file name:

```console
hacker@dojo:~$ echo >(rev)
/dev/fd/63
hacker@dojo:~$
```

Where did `/dev/fd/63` come from? `bash` replaced `>(rev)` with the path of the named pipe file that's hooked up to `rev`'s input! While the command is running, writing to this file will pipe data to the standard input of the command. Typically, this is done using commands that take output files as arguments (like `tee`):

```console
hacker@dojo:~$ echo HACK | rev
KCAH
hacker@dojo:~$ echo HACK | tee >(rev)
HACK
KCAH
```

Above, the following sequence of events took place:

1. `bash` started up the `rev` command, hooking a named pipe (presumably `/dev/fd/63`) to `rev`'s standard input
2. `bash` started up the `tee` command, hooking a pipe to its standard input, and replacing the first argument to `tee` with `/dev/fd/63`. `tee` never even saw the argument `>(rev)`; the shell _substituted_ it before launching `tee`
3. `bash` used the `echo` builtin to print `HACK` into `tee`'s standard input
4. `tee` read `HACK`, wrote it to standard output, and then wrote it to `/dev/fd/63` (which is connected to `rev`'s stdin)
5. `rev` read `HACK` from its standard input, reversed it, and wrote `KCAH` to standard output

last sol : `/challenge/hack 2> >(/challenge/the) > >(/challenge/planet)`
