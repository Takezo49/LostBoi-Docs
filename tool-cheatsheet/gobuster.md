# Gobuster

####

#### 🔹 Directory

* `gobuster dir -u http://example.com -w common.txt`
* `gobuster dir -u http://example.com -w common.txt -x php,html,txt`
* `gobuster dir -u http://example.com -w common.txt -s 200,301,302,403`
* `gobuster dir -u http://example.com -w common.txt -t 64`
* `gobuster dir -u http://example.com -w common.txt --wildcard`
* `gobuster dir -u http://example.com -w common.txt -o results.txt`
* `gobuster dir -u http://example.com -w common.txt -a "Mozilla/5.0" -c "SESSION=1"`

#### 🔹 DNS

* `gobuster dns -d example.com -w subdomains.txt`
* `gobuster dns -d example.com -w subdomains.txt -t 64 --no-error`
* `gobuster dns -d example.com -w subdomains.txt -i`
* `gobuster dns -d example.com -w subdomains.txt -r 8.8.8.8`
* `gobuster dns -d example.com -w subdomains.txt --wildcard`

#### 🔹 Vhost

* `gobuster vhost -u http://example.com -w vhosts.txt`
* `gobuster vhost -u http://example.com -w vhosts.txt --append-domain`
* `gobuster vhost -u https://example.com -w vhosts.txt --no-tls-validation`
* `gobuster vhost -u http://example.com -w vhosts.txt -H "User-Agent: Gobuster"`
* `gobuster vhost -u http://exampl.com -w /usr/share/wordlists/seclists/Discovery/DNS/combined_subdomains.txt -t 64 --no-error --append-domain`

#### 🔹 Fuzz

* `gobuster fuzz -u "http://example.com/FUZZ" -w common.txt`
* `gobuster fuzz -u "http://example.com/index.php?FUZZ=1" -w params.txt`
* `gobuster fuzz -u "http://example.com" -H "Host: FUZZ.example.com" -w subdomains.txt`
* `gobuster fuzz -u "http://example.com" -c "FUZZ=1" -w cookies.txt`

#### 🔹 Useful Flags

* `-o results.txt` (save output)
* `-q` (quiet)
* `-t 64` (threads)
* `--timeout 10s` (timeout)
* `--retry 5` (retries)
* `--delay 500ms` (delay between requests)
* `--proxy http://127.0.0.1:8080` (proxy)
* `--random-agent` (random UA)
* `-H "Header: value"` (custom header)
* `-x php,html,txt,bak` (extensions)
* `-s 200,301,302,403` (status codes)
* `-b 404,500` (exclude codes)
* `--wildcard` (wildcard handling)
