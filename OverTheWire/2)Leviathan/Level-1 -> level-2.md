Just use GDB to see the asm of the bin. Over there there are
strings being stored in the a register and in a later stage 
they are being checked if it is equal to the input provided.
The ans is "sex" 💀. You needn't worry about the `\n` as only
the first 3 chars are being checked . Then it will open a
shell with leviathan2 as the user from where you can easily
just check the password.

Another interesting way to find out the password was to use
`ltrace`.