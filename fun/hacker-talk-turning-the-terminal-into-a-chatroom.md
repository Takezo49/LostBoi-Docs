# “Hacker Talk: Turning the Terminal into a Chatroom”

So, i’ll Start with a simple way. And later Move on to Advanced Techniques.

### Using netcat(nc) <a href="#id-513e" id="id-513e"></a>

This is the main reason why it is called as “Swiss army knife” For terminal Networking.

So, the basic idea is simple. We setup a listener and Connect back to establish a connection and start texting.

<figure><img src="https://miro.medium.com/v2/resize:fit:468/1*2mx1HknwSFRRhIAgdoc-qA.png" alt="" height="326" width="468"><figcaption></figcaption></figure>

So we started a listener waiting for connection from anywhere on internet.

<figure><img src="https://miro.medium.com/v2/resize:fit:469/1*7oiBwEiihYsylCaeHAk2uw.png" alt="" height="291" width="469"><figcaption></figcaption></figure>

Specify the address of server(IP) and the Port Which the server is listening on.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*6PppMDMmIvH9ulMc0LMJjg.jpeg" alt="" height="394" width="700"><figcaption></figcaption></figure>

So, We have established a successful connection to Talk to each other.

It’s just a simple technique And very unsafe to rely on Since, the IP of client is getting leaked in hosts net cat interface. And anyone who is sniffing on the same WiFi can detect the communication.

So, This is just a simple and Fun way to get out of boredom and feel like a real hacker 😜.

STAY TUNED I’LL EXPLAIN VERY COMPLEX WAYS YOU CAN TRY IT OUT.
