`ps` by default lists the current processes running in the term.
The PID(process ID) is a number which uniquely identifies every
running process on the linux machine.
![[Pasted image 20250211173245.png]]
there is also the terminal on which it is running (pts/[0 1 2])
and the total amount of _cpu time_ that the process has eaten up
so far (since these processes are very undemanding, they have 
yet to eat up even 1 second!).

As `ps` is a very old utility, its usage is a bit of a mess. 
There are two ways to specify arguments.

**"Standard" Syntax:** in this syntax, you can use `-e` to list 
"every" process and `-f` for a "full format" output, including 
arguments. These can be combined into a single argument `-ef`.

**"BSD" Syntax:** in this syntax, you can use `a` to list processes 
for all users, `x` to list processes that aren't running in a 
terminal, and `u` for a "user-readable" output. These can be 
combined into a single argument `aux`.

These two methods, `ps -ef` and `ps aux` result in slightly 
different, but cross-recognizable output.

`kill` takes pid as its args and terminates that process

`ctrl  + z` suspends a program.`fg` can be used to resume it again.

You can access the exit code of the most recently-terminated 
command using the special `?` variable (don't forget to prepend 
it with `$` to read its value!)

The `s` part in place of the executable bit means that the program is executable _with SUID_. It means that, regardless of what user runs the program (as long as they have executable permissions), the program will execute as the owner user (in this case, the `root` user).
# Learn a bit more about bg and fg processes
