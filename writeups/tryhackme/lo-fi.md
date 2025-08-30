# Lo-Fi

<figure><img src="https://miro.medium.com/v2/resize:fit:765/1*DU73rQZKg-Vg0qafxW0W4w.png" alt="" height="286" width="612"><figcaption></figcaption></figure>

Lo-Fi is any easy machine which focuses on basic LFI techniques and exploit it to get the flag.

### Scanning <a href="#id-7bcb" id="id-7bcb"></a>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*iKIuVfDvFKKForWLXUXWwg.png" alt="" height="421" width="700"><figcaption></figcaption></figure>

only two open ports were found.

### Port 80 <a href="#id-47b0" id="id-47b0"></a>

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*dVPiL9Bx2e3VXCbX-kDy7w.png" alt="" height="397" width="700"><figcaption></figcaption></figure>

This is the face of the page.

### Gobuster <a href="#id-3ef0" id="id-3ef0"></a>

Running gobuster did’nt gave any directories which can be useful.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*fEbd9h29jYIWJwlJtSomvw.png" alt="" height="280" width="700"><figcaption></figcaption></figure>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*ImfIUpN-DHzKwF4XI_f9uA.png" alt="" height="232" width="700"><figcaption></figcaption></figure>

### LFI(Local File Inclusion) <a href="#id-139e" id="id-139e"></a>



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*4IXxJGPX75DnYbvszWZY2g.png" alt="" height="291" width="700"><figcaption></figcaption></figure>

fter messing with the source code i saw the href which is fetching a file from the server.

Two common readable files that are available on most back-end servers are /etc/passwd on Linux and C:\Windows\boot.ini on Windows.

I recommend to do Portswigger’s LFI module in which they explained How it’s is done.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*PLEtKZTRiJFVOJ9q4opcTw.png" alt="" height="367" width="700"><figcaption></figcaption></figure>

Started to fetch the file directly but it gave a messagethat hacker detected.

try adding ../ before the /etc/passwd file.

After adding three of them will get the contents of the file.



<figure><img src="https://miro.medium.com/v2/resize:fit:875/1*8MrcjiaSqj-5Tl7b-sIrKw.png" alt="" height="391" width="700"><figcaption></figcaption></figure>

Since they didn't ask any user flag and root flag the only flag we can get is flag.txt and do the same to get it.

<figure><img src="https://miro.medium.com/v2/resize:fit:769/1*0EHMgYsYXG2U0vDPjzb9BQ.png" alt="" height="390" width="615"><figcaption></figcaption></figure>

That’s it happy hacking.
