---
icon: forward
---

# Port Forwarding

### Cheat Sheet

```jsx
# the target host 192.168.0.100 is running a service on port 8888
# and you want that service available on the localhost port 7777
        
ssh -L 7777:localhost:8888 user@192.168.0.100
        
        
# remote port forwarding
# you are running a service on localhost port 9999 
# and you want that service available on the target host 192.168.0.100 port 12340
        
ssh -R 12340:localhost:9999 user@192.168.0.100
       
# Local proxy through remote host        
# You want to route network traffic through a remote host target.host
# so you create a local socks proxy on port 12001 and configure the SOCKS5 settings to localhost:12001
       
ssh -C2qTnN -D 12001 user@target.host# local port forwarding
       
          
```
