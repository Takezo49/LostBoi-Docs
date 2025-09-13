---
icon: gauge-simple-max
---

# Linux Quick Enum

## Linux quick enum

Nice — here’s a compact, practical checklist of **post-initial-access enumeration commands** you can run on a box to quickly map processes, listeners, network info, users, services, scheduled jobs, mounts, and important files. I split it by **Linux** / **Windows** and add a few cross-platform tips (how to save output, quick ordering). Use these only on systems you’re authorized to test.

## Quick tips before you run

* Prefix long runs with `sudo` if needed.
* Save everything: `COMMAND 2>&1 | tee ~/enum/<host>-<command>.txt`
* Run non-destructive commands first (discovery), then deeper checks.
* Be mindful of logs and OPSEC on real engagements.

***

## Linux — quick enumeration

System & identity

```
uname -a
cat /etc/os-release
hostnamectl
whoami
id
getent passwd | tail -n +1
uptime
last -n 5

```

Processes & runtime

```
ps auxf
pstree -apl
top -b -n1 | head -n 40
# if available:
htop

```

Network & listeners

```
ss -tulpen       # preferred modern tool
netstat -tulpen  # if ss unavailable
lsof -i -P -n    # open network files
ip addr show
ip route show
arp -a

```

Open files, mounts & disks

```
df -h
mount | column -t
lsblk
find / -maxdepth 2 -type d -name '*log*' 2>/dev/null

```

Services & scheduled jobs

```
systemctl list-units --type=service --state=running
ps -ef | grep -i cron
crontab -l 2>/dev/null || true
ls -la /etc/cron* /var/spool/cron/crontabs 2>/dev/null

```

Users, groups & sudo

```
getent passwd
cut -d: -f1,3,4,6,7 /etc/passwd
sudo -l  # what the current user can run (if sudo present)
cat /etc/group

```

Files of interest & quick content checks

```
ls -la ~/
find /home -maxdepth 2 -type f -iname '*id_rsa*' 2>/dev/null
grep -Ri "password" /etc 2>/dev/null || true
# Search for private keys and configs (authorized use only)

```

Binary & package info

```
which python3 bash perl
ldd /usr/bin/somebinary  # inspect dependencies (if needed)
dpkg -l | head -n 40      # Debian/Ubuntu
rpm -qa | head -n 40     # RHEL/CentOS

```

Privilege-escalation surface (non-destructive checks)

```
find / -perm -4000 -type f -exec ls -ld {} \\; 2>/dev/null   # SUID files (scan-only)
find / -type d -perm -2 -ls 2>/dev/null                    # world-writable dirs

```

***

##

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
