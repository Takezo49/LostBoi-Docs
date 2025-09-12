# Fluffy

#### Fluffy-HTB

\


#### **Recon**

\


<figure><img src="https://cdn-images-1.medium.com/max/800/1*WOkRFNGGWbrho4Q-nWgI9g.png" alt=""><figcaption></figcaption></figure>

I noticed that DNS plus, Kerberos and Ldap are running so understood that it’s an active directory machine. Since we got domain names ass them into the etc/hosts files.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*mxiLjCq4glHSMnf31osoqA.png" alt=""><figcaption></figcaption></figure>

**SMB**&#x20;

As we have already given creds we’ll check the presence of this user using netexec.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*niev6k_g4PWN4tMyPPKBow.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://cdn-images-1.medium.com/max/800/1*Yp0xdcxp8qsXSaZrYp686g.png" alt=""><figcaption></figcaption></figure>

IT share was a bit off so connected to it.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*X_eyvy3YZ4ywhCEBO1RVUw.png" alt=""><figcaption></figcaption></figure>

These are all the files in IT Share. On Upgrade\_Notice.pdf we caught this.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*NqA9C5lMOCPQbUqnma7rlQ.png" alt=""><figcaption></figcaption></figure>

These are the list of vulnerabilities which should be patched as soon as possible. There are two critical vulns you can go with any of the cve im going exploit CVE-2025–24071.

**CVE-2025–24071**

\


This vuln is on windows 10. What happens is that when you send a zip file to the victim and victim extracts the file you sent his ntlm hash can be leaked via responder.

[https://github.com/Marcejr117/CVE-2025-24071\_PoC.git](https://github.com/Marcejr117/CVE-2025-24071_PoC.git) go through it if want to understand how this exploit works.

So first we have to create a zip file and transfer it into victim machine. We use SMB to do it.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*PHwBzDt_Whw8N2nhCwRvGw.png" alt=""><figcaption></figcaption></figure>

My advice is to start the responder in another terminal before u put the exploit.zip file into the victim machine so that we cannot miss the ntlm hash.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*9MyEE5Rb_e_6g5DNNqFEZw.png" alt=""><figcaption></figcaption></figure>

We have sucessfully recieved the ntlm hash of the user agile so time to crack the hash in our local machine using hashcat.

```
hashcat -m 5600 hash.txt <wordlist>
```

<figure><img src="https://cdn-images-1.medium.com/max/800/1*7JPVzFB7jX6ka7Ngt0X-gQ.png" alt=""><figcaption></figcaption></figure>

We got the password of p.agila:prometheusx-303. We’ll check the validity of the user.

<figure><img src="https://cdn-images-1.medium.com/max/800/1*Pm9u5Rzym9v-hlnIj3RYCA.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://cdn-images-1.medium.com/max/800/1*aoVVqLEzbrQ3eKV0MPhtcw.png" alt=""><figcaption></figcaption></figure>

Take your time and understand it we can observe that p.agila is the member of the service accounts manager and has genericall to service accounts. So we can perform shadow credential attack.

So the first step for performing this attack is to make agila the memeber of service accounts so that we can move forward SCA.

1. &#x20;First we add agila to the members of Service Accounts.We use bloodyAD to do this.

```
bloodyAD --host DC01.fluffy.htb -d fluffy.htb -u "p.agila" -p "prometheusx-303" add groupMember "Service Accounts" "p.agila"
```

<figure><img src="https://cdn-images-1.medium.com/max/800/1*Xyf1pOxaww80RPaqGs_b2Q.png" alt=""><figcaption></figcaption></figure>

We can verify if it was added or not using nxc and dump the data into bloodhound.

```
nxc ldap dc01.fluffy.htb -u 'p.agila' -p 'prometheusx-303' --bloodhound --collection All --dns-tcp --dns-server 10.10.11.69
```

<figure><img src="https://cdn-images-1.medium.com/max/800/1*l0ob14cmtf1Q8GkPFe7hHQ.png" alt=""><figcaption></figcaption></figure>

So we have successfully added agila to Service Accounts.

**The full version will be released after it is no longer on active machines, according to HackTheBox policy.**
