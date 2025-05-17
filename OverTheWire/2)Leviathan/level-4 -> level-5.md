`ls -la` would reveal a `.trash` dir which has a `bin` exe. Upon 
running `ltrace` on it, it's clear that it's reading the passwd
file for the 5th level and gives the binary equivalent of 
the text. Just convert that binary to ascii using an online
tool.