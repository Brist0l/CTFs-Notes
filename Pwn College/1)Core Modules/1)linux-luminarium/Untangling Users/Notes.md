When you enter a password for `su`, it compares it against the stored password for that user. These passwords _used_ to be stored in `/etc/passwd`, but because `/etc/passwd` is a globally-readable file, this is not good for passwords, these were moved to `/etc/shadow`.

`cracking passwords` is an interesting one.