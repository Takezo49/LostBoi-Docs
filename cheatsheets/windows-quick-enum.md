---
icon: gauge-simple-high
---

# Windows Quick enum

## Windows quick enum

## Windows — quick enumeration (CMD / PowerShell)

Basic system & identity

```
whoami
whoami /groups
whoami /priv
systeminfo
hostname
query user

```

Processes & services

```
tasklist /v
wmic process get ProcessId,Name,CommandLine
Get-Process | Format-Table -AutoSize   # PowerShell
sc queryex type= service state= all
Get-Service | Where-Object {$_.Status -eq 'Running'}

```

Network & listeners

```
netstat -ano
Get-NetTCPConnection -State Listen   # PowerShell (if available)
ipconfig /all
route print
arp -a

```

Scheduled tasks & autoruns

```
schtasks /query /fo LIST /v
Get-ScheduledTask | Where-Object {$_.State -eq 'Ready' -or $_.State -eq 'Running'}
reg query "HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run" /s

```

Filesystem & users

```
dir C:\\Users /a
icacls C:\\Some\\Path
net user
net localgroup administrators

```

Installed programs & patches

```
wmic product get name,version
Get-HotFix
reg query "HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall" /s

```

Registry & creds (discovery only)

```
reg query "HKLM\\SYSTEM\\CurrentControlSet\\Services" /s
reg query "HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon" /s
# avoid dumping SAM/SECURITY unless explicitly authorized

```

***

## Network / Pivoting checks (both)

```
# List routing & forwarding
ip route show         # Linux
route print           # Windows

# Check other hosts on same subnet
nmap -sn <subnet>/24   # careful/authorized use only

# Check DNS configuration
cat /etc/resolv.conf   # Linux
ipconfig /all          # Windows

```

***

## Evidence collection & safe logging

* Save outputs to a directory: `mkdir -p ~/enum && COMMAND |& tee ~/enum/COMMAND.txt`
* Timestamp each file: `date -u +"%Y%m%dT%H%MZ" > ~/enum/timestamp.txt`
* Gather basic artifacts: `uname -a > os.txt; id > id.txt; ss -tulpen > listeners.txt`

***

## Suggested quick order to run (non-destructive)

1. System basics (`uname`, `systeminfo`, `hostname`, `uptime`, `whoami`)
2. Processes & services (`ps aux`, `tasklist`, `systemctl list-units`)
3. Network & listeners (`ss/netstat`, `ip addr`, `netstat -ano`)
4. Users & sudo/sessions (`getent passwd`, `sudo -l`, `whoami /groups`)
5. Scheduled jobs & autoruns (`crontab -l`, `schtasks`)
6. Filesystem checks & key lookups (`ls -la ~`, find for keys)
7. Package list & patches (`dpkg -l` / `wmic product`, `Get-HotFix`)
8. Save outputs and review.
