# Page 1

### What is port Forwarding?

Port forwarding is the process of redirecting network traffic from one port or address to another.

Example : Let’s consider a situation you have compromised a machine and identified that a different application is running on [localhost](http://localhost) on a different port. Now you want to Forward that Port to your Attacker Machines.

#### USING SSH

Since you have access to compromised machine you can forward the local address and port to your machine

```jsx
ssh -L 1234:localhost:8000 victim@<ADDRESS>
```

-L for local port Forward.

[localhost:8000](http://localhost:8000) is the address and port which the application is running on victim machine.

1234 is the port which you want the traffic to be redirected too.

victim@\<ADDRESS> is the address of your victim machine.

Now you can access locally running application in attackers [localhost:1234](http://localhost:1234).
