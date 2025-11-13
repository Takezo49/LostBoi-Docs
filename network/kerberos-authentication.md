# Kerberos Authentication

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



### **Transaction 1:** <a href="#b71b" id="b71b"></a>

> — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —\
> &#xNAN;_&#x43;lient sends 1 message to Authentication Server (AS)._\
> &#xNAN;_**Message 1** is a plaintext contains_ user name/id, service name/id, requested lifetime of a TGT (Ticket Granting Ticket)_._\
> &#xNAN;_— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —_\
> &#xNAN;_&#x4F;n receiving the message in Authentication Server, it will search for user name/id to validate if its an existing user or not in the_ Key Distribution Center (KDC) databas&#x65;_._\
> &#xNAN;_&#x49;f it finds the user, AS will send 2 messages to client._\
> &#xNAN;_**Message**_ _**1**_ _**encrypted** with client secret key contains_ Ticket Granting Server (TGS) Id, timestamp, lifetime, TGS Session Ke&#x79;_._\
> &#xNAN;_**Message 2**_ _**encrypted** with TGS secret key contains_ user name/id, TGS Id, timestamp, user IP address, lifetime, TGS Session Ke&#x79;_. This message is called **Ticket Granting Ticket (TGT).**_\
> &#xNAN;_&#x41;s client receives the 2 messages from AS, it tries to decrypt the Message 1 with its_ Client Secret Key (users password hashed with salt)_, if the user has not entered the right password the process will terminate here only but lets assume the user enters correct password._\
> &#xNAN;_&#x53;o now client has Message 1 data which includes a TGS Session Key but cannot decrypt Message 2 since it is encrypted with a different key._

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:627/1*VKIvFstCMG033woMIh6pKw.png" alt="" height="480" width="700"><figcaption></figcaption></figure>

### **Transaction 2:** <a href="#f940" id="f940"></a>

> _— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —_\
> &#xNAN;_&#x43;lient will send the Message 2 with another 2 messages to TGS, in total 3 messages._\
> &#xNAN;_**Message 1** contains same data as (_&#x4D;essage 2 with encryption) TGT _received from AS._\
> &#xNAN;_**Message 2** is a plaintext contains_ service name/id, lifetim&#x65;_._\
> &#xNAN;_**Message 3**_ _**encrypted** with TGS session key contains_ user name/id, timestam&#x70;_. This message is called **User Authenticator**._\
> &#xNAN;_— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —_\
> &#xNAN;_&#x4F;n receiving these messages in TGS, it will check if service name/id from Message 2 in KDC database exists, if it exists it will decrypt the Message 1 with TGS Secret Key and recover TGS session key._\
> &#xNAN;_&#x54;his TGS Session Key will be used to decrypt Message 3 (User Authenticator)._\
> &#xNAN;_&#x54;GS also maintains a TGS cache where it stores all the received previous User Authenticators so that replay attacks can prevented and the TGS will only send if it does not find any record of the User Authenticator it just received and it will store the current the User Authenticator in the record for later receiving messages._\
> &#xNAN;_&#x54;GS will send 2 messages to client on successful decryption and validation of messages received._\
> &#xNAN;_**Message 1**_ _**encrypted** with TGS Session Key contains_ service name/id, timestamp, lifetime, Service Session Ke&#x79;_._\
> &#xNAN;_**Message 2**_ _**encrypted** with Service Secret Key contains_ user name/id, service name/id, timestamp, user IP address, lifetime, Service Session Ke&#x79;_._\
> &#xNAN;_&#x43;lient will decrypt Message 1 with TGS session key which it recovered in Transaction 1 and get a Service Session Key but won’t be able to decrypt Message 2 since it does not have a Service Secret Key._

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:627/1*Wu5XBB4jLgk7oREOQl4Snw.png" alt="" height="699" width="700"><figcaption></figcaption></figure>

### **Transaction 3:** <a href="#id-6f3b" id="id-6f3b"></a>

> _— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —_\
> &#xNAN;_&#x43;lient will send 2 messages to Application Server._\
> &#xNAN;_**Message 1** contains same data as_ Message 2 encrypted with Service Secret Key _received from TGS._\
> &#xNAN;_**Message 2 encrypted** with Service Session Key contains a_ new User Authenticato&#x72;_._\
> &#xNAN;_— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —_\
> &#xNAN;_&#x41;pplication Server will decrypt Message 1 Service Secret Key and get a Service Session key from that message. It will decrypt the Message 2 with the Service Session Key and gets a User Authenticator message._\
> &#xNAN;_&#x41;pplication Server also maintains a service cache where it store the previous received User Authenticators so that it can compare it with currently received User Authenticator to prevent replay attacks. And if there is not record then it will add the currently received User Authenticator in the cache._\
> &#xNAN;_&#x41;pplication Server will send a single message to Client._\
> &#xNAN;_**Message 1 encrypted** with Service Session Key contains_ service name/id, timestam&#x70;_. This message is called **Service Authenticator**._\
> &#xNAN;_&#x4F;n receiving the message in client it will decrypt the Message 1 with the same Session Key that it already has and check if the service name is same as the requested service._\
> &#xNAN;_&#x43;lient also maintains a Client cache where is stores the previously received Service Authenticator for future use._

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:627/1*mkrcIq9wG0Kl14ltrDRErA.png" alt="" height="515" width="700"><figcaption></figcaption></figure>

And now with that Service Session Key the Client and Application server continue interacting without needing to share the actual password to the each other.\
It works on a symmetric key authentication but the keys are not provided publicly but instead from a third party server hosted in the enterprise environment which is trusted by both users and application servers.

From the above message transactions we can conclude that client and application server need to **trust a third party which is Kerberos server where it has&#x20;**_**stored hashed secret keys of both users & application**_**&#x20;and grants tickets** to each of them.
