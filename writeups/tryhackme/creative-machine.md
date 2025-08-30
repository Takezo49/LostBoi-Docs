# Creative Machine

It’s an Easy machine which can be solved with some basic reconnaissance and finding a subdomain which has a functionality to test a URL weather it is dead or alive which will lead us to exploit SSRF vulnerability and get the ssh key to a user and will end with a simple misconfiguration to get root.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*O0yLjYZEO6GZVRk5Mxnm4A.jpeg" alt="" height="438" width="700"><figcaption></figcaption></figure>

### NMAP <a href="#c6b6" id="c6b6"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*u4PkULphF-g7UrVIRCZs4A.png" alt="" height="497" width="700"><figcaption></figcaption></figure>

Got two open port 22-ssh,80-http.

<figure><img src="https://miro.medium.com/v2/resize:fit:524/1*Va1OEF6yT4fxVqg2v68wzg.png" alt="" height="60" width="419"><figcaption></figcaption></figure>

### Port-80 <a href="#id-1ac3" id="id-1ac3"></a>

Looking at the page i didn’t got anything much but some rabbit-holes.

#### Gobuster <a href="#id-3781" id="id-3781"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*RJjOrpljrGDSVydA1_GJDg.png" alt="" height="238" width="700"><figcaption></figcaption></figure>

Got only one directory which was not useful.

#### SUBDOMAIN’S <a href="#f5dd" id="f5dd"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*WAPQKrqhf95ifDi9q24yPQ.png" alt="" height="313" width="700"><figcaption></figcaption></figure>

Then we got a subdomain beta.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*4pyNvotd4drxmxOJo0x-YA.png" alt="" height="202" width="700"><figcaption></figcaption></figure>

It has a function to test weather the page is dead or alive. When i intercepted the request it was using post method to the server to check if page is alive or not.

### SSRF — (Server Side Request Forgery) <a href="#fa68" id="fa68"></a>

Immediately tested for ssrf.Noticed that it is running a service on loopback address localhost or 127.0.0.1.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*KP-WEATjalLROaltT0xCDg.png" alt="" height="491" width="700"><figcaption></figcaption></figure>

I’ve used burp’s intruder to fuzz the ports you can also use wfuzz to do this.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*efM8jUDLTPGUFdXrAwnqWg.png" alt="" height="343" width="700"><figcaption></figcaption></figure>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*vywdXQUwRzx_WPaqcTxrEw.png" alt="" height="277" width="700"><figcaption></figcaption></figure>

Found two open ports 80 and 1337. And one of them was listing files of the machine.

### Port -1337 <a href="#id-24be" id="id-24be"></a>

<figure><img src="https://miro.medium.com/v2/resize:fit:533/1*kSGscNicjywB8KWOEW_jEg.png" alt="" height="660" width="426"><figcaption></figcaption></figure>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*iR9Ep_lrh1K2MPfqTHlthw.png" alt="" height="386" width="700"><figcaption></figcaption></figure>

got user saad.

### SSH-PORT 80 <a href="#id-904b" id="id-904b"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*zJFUZ5qiFBxYrWL6Yx07FQ.png" alt="" height="397" width="700"><figcaption></figcaption></figure>

And we got a private ssh key.

<figure><img src="https://miro.medium.com/v2/resize:fit:806/1*ZX5TwxpENgDNsGVYj8_2Ww.png" alt="" height="209" width="645"><figcaption></figcaption></figure>

upon using , it was asking for passphrase for the key.

### ssh2john <a href="#a98f" id="a98f"></a>

simply crack it using john.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*QNVGN4_QIiP7YmMnDfd1pg.png" alt="" height="356" width="700"><figcaption></figcaption></figure>

### ENUMARATION <a href="#id-8a95" id="id-8a95"></a>

Started enumerating with linpeas by transfering it into the machine from my local machine and we will get a CVE but it was for kernel 4.10 to 5.1.17 ours was 5.4.XX. So it will not work.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*ygFKoCsaeQ7WfPvAcxyncQ.png" alt="" height="418" width="700"><figcaption></figcaption></figure>

checking bash\_history file will get you password for the user saad.

<figure><img src="https://miro.medium.com/v2/resize:fit:524/1*LQ6ukIctFSoHHDqG9fhWiA.png" alt="" height="191" width="419"><figcaption></figcaption></figure>

### PRIVILEGE ESCALATION <a href="#id-6c9a" id="id-6c9a"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*-2BHDVuyZeMFQbYeJQULeQ.png" alt="" height="69" width="700"><figcaption></figcaption></figure>

/usr/bin/ping was allowed to run with sudo without any password but we cant’d do anything with that and noticed env\_keep=LD\_PRELOAD.

### LD\_PRELOAD <a href="#a722" id="a722"></a>

#### What is LD\_PRELOAD? <a href="#id-6245" id="id-6245"></a>

It’s a tool in Linux that lets you “trick” a program into using _your own version_ of a function (from a shared library) instead of the original one.

In simple words it’s like adding your own recipe instead of the original recipe without telling chef.

It’ll load our own custom shared object(.so) before loading the original one.

[https://www.hackingarticles.in/linux-privilege-escalation-using-ld\_preload/](https://www.hackingarticles.in/linux-privilege-escalation-using-ld_preload/) gives clear explanation on how it works.

So start by creating a file which will get us a shell using c.

<figure><img src="https://miro.medium.com/v2/resize:fit:256/1*Kcn2NY38Nx5djP9Nc69c2w.png" alt="" height="191" width="205"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:528/1*aMBysTJSZe2nVpNbGNZZkg.png" alt="" height="69" width="422"><figcaption></figcaption></figure>

Compile it to generate a shared object with .so extension if it is windows then with .dll extension.Then Run it by specifying the .so file we created and at last the program we want to use.

> note : We need a program to use LD\_PRELOAD in this case we got ping with sudo permissions to successfully get the shell.

<figure><img src="https://miro.medium.com/v2/resize:fit:558/1*zMMmoggiMYHcvCMLMdSFXg.png" alt="" height="92" width="446"><figcaption></figcaption></figure>

Then load our custom file before loading libraries of the ping.

That’s it, Happy Hacking ; )
