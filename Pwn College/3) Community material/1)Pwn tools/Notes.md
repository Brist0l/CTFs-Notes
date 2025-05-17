https://docs.pwntools.com/en/stable/

basic structure of a a script is:
``
```
from pwn import *

# Set architecture, os and log level
context(arch="amd64", os="linux", log_level="info")

# Load the ELF file and execute it as a new process.
challenge_path = "/challenge/pwntools-tutorials-level0.0"
p = process(challenge_path)

payload = b'FIXME'
# Send the payload after the string ":)\n###\n" is found.
p.sendafter(":)\n###\n", payload)

# Receive flag from the process
flag = p.recvline()
print(f"flag is: {flag}")
```

![[Pasted image 20250326100236.png]]
the `p` functions pack the int into hex with the number of 
bytes specified .

```from pwn import *

context(arch="amd64",os="linux",log_level="debug")

challenge_path = "/challenge/pwntools-tutorials-level1.0"
p = process(challenge_path)

payload = p32(0xdeadbeef) + b'\n'
p.sendafter(":)\n###\n", payload)

flag = p.recvline()
print(flag)
```
