HTTP is the lingua franca of the internet

The request for HTTP is like:![[Pasted image 20250304095102.png]]
The response is:![[Pasted image 20250304095124.png]]
The status codes are defined as:![[Pasted image 20250304095228.png]]
![[Pasted image 20250304095520.png]]

The GET request get "all" of the info including the content and the header, whereas the POST request only gets the header 
only.
The POST request pushes some data on the server whereas GET
literally gets some data from the server.

metadata is the data about the data, it is sent by HTTP using
headers.

`curl` can be used to send request:
`curl -A "Firefox" www.google.com` will change the user-agent to firefox
before sending
`curl --head -A "Firefox" www.google.com` will only get the header as 
firefox

Netcat's basic usage involves two arguments: the _hostname_ (where the server is listening on, such as [www.google.com](http://www.google.com) for Google), and the port (the standard HTTP port is 80).
![[Pasted image 20250304110019.png]]
you would need to use `-e` with echo or else it won't understand
the CRLF(control line feed)(`\r\n`)

Unfortunately, most of the modern internet runs on the infrastructure of a handful of companies, and a given server run by these companies might be responsible for serving up websites for dozens of different domain names. How does the server decide which website to serve? The `Host` header.

The `Host` header is a _request_ header sent by the client (e.g., browser, curl, etc), typically equal to the domain name entered in the HTTP request. When you go to `https://pwn.college`, your browser automatically sets the `Host` header to `pwn.college`, and thus our server knows to give you the `pwn.college` website, rather than something else.

To set headers in `requests`, pass in a dictionary to "headers"
in the function of the method desired. eg:![[Pasted image 20250304161325.png]]

![[Pasted image 20250306103141.png]]
By default the port is set to 80.
Whenever you need to add a space or any other kind of char in 
the host then you need to use url encodings.
0x20 is for space.

There are parameters which can be passed into the URL![[Pasted image 20250306104340.png]]
They aren't like normal programming language arguments as they
can't be called by anyone anywhere. But over here it can be 
done.Multiple parameters can be passed by using `&`.
![[Pasted image 20250306105100.png]]

`-d` will do POST
![[Pasted image 20250306141017.png]]

POST using nc:
![[Pasted image 20250306143006.png]]

curl cookie program:
`curl http://127.0.0.1 -v --cookie "207fc7cba4ddda4bdd3b3978ca3c9157" -L`

Cookies are used for specifying "state". Once a user has 
logged in a server , the server sends a response with a title
`Set-Cookie` and it has a value and whenever the client wants to 
communicate with the same state the client will give the same
cookie in the `Cookie` field.
![[Pasted image 20250310094727.png]]
![[Pasted image 20250310094737.png]]
![[Pasted image 20250310094746.png]]
![[Pasted image 20250310094808.png]]
