# U.A. High School

<figure><img src="https://miro.medium.com/v2/resize:fit:759/1*_DqvmjFGk-9x6jv4vXy5qg.png" alt="" height="146" width="607"><figcaption></figcaption></figure>

#### NMAP <a href="#bdfe" id="bdfe"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*nrWKoVm0fWw56o24i9Ku9w.png" alt="" height="298" width="700"><figcaption></figcaption></figure>

### Port 80-enum <a href="#id-8c67" id="id-8c67"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*pHgK8nTTrVG4UWlnBqI-Dw.png" alt="" height="498" width="700"><figcaption></figcaption></figure>

The only vector we got here is contact page. But looking at the source code of the page the contact form doesn't do anything.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*qKzo2RskTs9tqZteZrF9iA.png" alt="" height="274" width="700"><figcaption></figcaption></figure>

we can see that in action parameter.

So the next options is to look at the directories.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*ZzgNMupfIaf0LUP9PvYKpQ.png" alt="" height="234" width="700"><figcaption></figcaption></figure>

> Got only one directory /assets.

But it was just a blank page. So i ran gobuster again and i’ve noticed that it is running using php so i used wordlist which has some common php files in it.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*zYmWW5X_2hS8hsboDT5Gew.png" alt="" height="238" width="700"><figcaption></figcaption></figure>

we got 200 status code on /index.php.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*90D0qcpeDhFBX-9rV6Qtvg.png" alt="" height="511" width="700"><figcaption></figcaption></figure>

It was just a blank page that’s unusual. I’ve was thinking it is running a shell using php.\
And got an php endpoint.

<figure><img src="https://miro.medium.com/v2/resize:fit:833/1*_nWD8Nu0jE6QGh-F7NgZLA.png" alt="" height="115" width="666"><figcaption></figcaption></figure>

Running cmd=ls gave an output but it base64 encoded.

<figure><img src="https://miro.medium.com/v2/resize:fit:815/1*3OtcTT3GalScDeyWwBpGlw.png" alt="" height="162" width="652"><figcaption></figcaption></figure>

### Foothold <a href="#id-1d1d" id="id-1d1d"></a>

Bash shells were not working So used netcat shells to get it.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*-f_4e0c1orpezXAp3OHqYQ.png" alt="" height="366" width="700"><figcaption></figcaption></figure>

Setup a reverse shell and run the shell.

### Local Enumeration <a href="#id-55f4" id="id-55f4"></a>

There is only one user deku.

There is passphrase text file in /var/www/Hidden\_Content Directory.

<figure><img src="https://miro.medium.com/v2/resize:fit:819/1*aIzrHEUX5BAKK4dxyVccXw.png" alt="" height="133" width="655"><figcaption></figcaption></figure>

> AllmightForEver!!!

<figure><img src="https://miro.medium.com/v2/resize:fit:661/1*IL6ipLnh94QWI4BpWS7MsA.png" alt="" height="205" width="529"><figcaption></figcaption></figure>

And the assets directory was standing out with unusual permissions on it.

<figure><img src="https://miro.medium.com/v2/resize:fit:666/1*G_5c3wKHWlpQjr4YIRZM_Q.png" alt="" height="381" width="533"><figcaption></figcaption></figure>

This explains why we were able to execute commands on the application using index.php. Taking look at the code, it is getting the command and executing it and encoding the output in base64.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*keg6yFlCzLaLZWF18pBK6A.png" alt="" height="317" width="700"><figcaption></figcaption></figure>

Images directory was containing two images and one of them was containing data. So moved it onto my local machine to examine it.

After fixing it by using magicbytes tool we can see the image.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*rWAaZ3UPUjRpWJeh2wlrag.png" alt="" height="282" width="700"><figcaption></figcaption></figure>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*8gElwjDiSC1ZzisHfWaT1Q.png" alt="" height="461" width="700"><figcaption></figcaption></figure>

Given the passphrase i was thinking that it is some kind of Steganography machine so ran steghide using the given passphrase and we got the creds of the user deku.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*XOqXskhlPQuXHMhlRLIQYg.png" alt="" height="194" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:500/1*u659A1Xjty9VKlyL6L3c8A.png" alt="" height="78" width="400"><figcaption></figcaption></figure>

### Privilege Escalation <a href="#id-96ea" id="id-96ea"></a>

Got a shell as user deku using ssh.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*zEGeUXNqYgQNtb_BIL3iog.png" alt="" height="122" width="700"><figcaption></figcaption></figure>

And the user deku can run feedback.sh as root without password.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*Zdfh-IkvKyn3JynDbXf2zg.png" alt="" height="302" width="700"><figcaption></figcaption></figure>

And the user deku can run feedback.sh as root without password.

we can use eval function to get root so basically eval is just running the input which is given by the user.

> deku ALL=NOPASSWD: ALL >> /etc/sudoers

Added this to the sudoers file.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*7tM_sdynmNwLHefnDGUMMw.png" alt="" height="214" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:811/1*oXBnVrmY-e5fbdo7wC0UMA.png" alt="" height="300" width="649"><figcaption></figcaption></figure>

That’s it, Happy Hacking ; )
