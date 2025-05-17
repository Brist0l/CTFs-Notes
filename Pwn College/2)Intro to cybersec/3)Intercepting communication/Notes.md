![[Pasted image 20250329114025.png]] If the packet needs to go to host A to B then it would first 
go to the bridge from vethA and then go to vethB and then exit
to eth0 of HostB. 
![[Pasted image 20250329114120.png]]
The packet looks like this , `Type` will be covered later.
If you want to broadcast to all the hosts connected then 
set the destination as `ff:ff:ff:ff:ff:ff`

# Challenges

## 1)connect
To connect to a remote host use `nc`. 
ans : `nc 10.0.0.2 31337`
## 2)send
![[Pasted image 20250329120913.png]]
Msgs can be sent also by typing and "entering".
or
![[Pasted image 20250329121001.png]]

## 3)shutdown
just press ctrl+d

## 4) Listen
From your host at 10.0.0.1, listen on port 31337 for a 
connection from the remote host at 10.0.0.2.

Once a connection is established, that connection is 
bidirectional, meaning that both sides can send and receive 
data. However, to actually establish the connection, one side 
must listen for incoming connections, and the other side must 
connect to that listener. This time, unlike before, you are 
the listener.

use `-l` to listen

## 5) Scan 1
[[2)Subnets]]
I need to ping 256 hosts. So just use a shell script:
`for i in $(seq 256); do timeout 1 ping 10.0.0.$i; done`
then just find the one who responds and connect to that host.

## 6) Scan 2

To scan lots of IPs use tools like `nmap`

## 7) Monitor 1
Used wireshark and looked inside the packet for the flag

## 8) Monitor 2

## 9) Sniffing Cookies
Used wireshark to get the cookies and then:![[Pasted image 20250330100243.png]]

## 10) Networking config 
I can change my IP address . If i run tcpdump i can see :
![[Pasted image 20250331162639.png]]
Now i become 10.0.0.3, using: 
![[Pasted image 20250331162800.png]]
Then I just listened on it:
![[Pasted image 20250331162825.png]]

## 11) Firewall 1

![[Pasted image 20250331165936.png]]
The mistake which i did before was that i set the source, so
it was blocking only the input packets from 10.0.0.1

## 12) Firewall 2
`iptables -A INPUT -p tcp --dport 31337 -s 10.0.0.3 -j DROP`

## 13) Firewall 3
![[Pasted image 20250401192922.png]]

## 14) DOS 1
After reading the src code, I found out that the client sends
"Hello, World" to the server so i just send that same messasge
after connecting to that server using nc.
## 15) DOS 2
## 16) DOS 3

## 17) Ether

In this I needed to craft an Ethernet packet . So for that
I will use scapy. 
![[Pasted image 20250403092054.png]]
This made a packet with just an ether layer. Now i need to set
the type to 0xFFFF , so i will do  `my_frame[0].type = 0xFFFF` , same
with the dst and src . For src i will get the mac using `ip addr`
and for the dst i will use `arping -c 1 10.0.0.2` and then use 
`sendp(my_frame)` to send the packet and I will get the flag.
## 18) IP
Same as above but this time `my_frame` would be `IP()` and instead 
of `sendp` i will use `send`.

## 19) TCP
same as above but I would need to layer it over IP() so that i 
can  tell it where it should go . I can do it by `my_frame = 
`IP()/TCP()`

## 20) TCP Handshake
## 21) UDP
Just cat the /challenge/run and you will find that you need 
to send a UDP packet to 10.0.0.2 and then just recv the flag.

```import socket

UDP_IP = "10.0.0.2"
UDP_PORT = 31337
MESSAGE = "Hello, World!\n"

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
print("socket created")
sock.sendto(MESSAGE.encode(), (UDP_IP, UDP_PORT))

print(sock.recvfrom(1024)[0])
```
## 22) UDP 2
Same as this I just need to bind to a certain socket .
## 23) UDP Spoofing 1
```py
import socket

UDP_IP = "10.0.0.2"
UDP_PORT = 31338
MESSAGE = "FLAG"

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
sock.bind(("10.0.0.1", 31337))
print("socket created")
sock.sendto(MESSAGE.encode(), (UDP_IP, UDP_PORT))

print(sock.recvfrom(1024)[0])
```

## 24) UDP Spoofing 2
```import socket

UDP_IP = "10.0.0.2"
UDP_PORT = 31338
MESSAGE = "FLAG:10.0.0.1:6969"

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
print("socket created")
sock.bind(("10.0.0.1", 31337))
print("socket binded")
sock.sendto(MESSAGE.encode(), (UDP_IP, UDP_PORT))
print("message sent")
```
I need to run the server in the same env , not split term.