---
layout: default
title: LFI
parent: CyberSecurity
grand_parent: Notes
---

## LFI

### Meaning:

`LFI` means `Local File Inclusion` and is a vulnerability found in various web applications that allows attackers to include and read local files on the server.
These files could contain sensitive data such as cryptographic keys, databases that contain passwords, and other pivate data.

### Example:

In `PHP` the following functions cause these kind of vulnerabilites:

- `include`
- `require`
- `include_once`
- `require_once`

Vulnerable `PHP` code example, which reads a local file allows editing that request to load another file perhaps not meant for viewing by the attacker.

```php
<?PHP 
	include($_GET["file"]);
?>
```

### Risks: 

Makes it possible to read senstive data if you have readable permission on files.
Also in some cases an LFI vulnerability could be chained to perform `RCE` (`Remote Code Execution`).
If we can inject or write to a file on the system, we can take advantage of `LFI` to get `RCE`. 

### Identifying / Testing:

An entry point to the web application attack types could be `HTTP` `GET` or `POST` parameters that pass an argument or data to the web application.

```
http://example.thm.labs/index.php?file=welcome.txt
```

Once you find an entry point, we need to understand how this data could be processed within the application. After this point you can start testing for certain vulnerability types using manual or automated tools.

### Linux system files with sensitive information: 

```
/etc/issue 
/etc/passwd 
/etc/shadow 
/etc/group 
/etc/hosts 
/etc/motd 
/etc/mysql/my.cnf 
/proc/[0-9]*/fd/[0-9]* (first number is the PID, second is the filedescriptor)
/proc/self/environ 
/proc/version 
/proc/cmdline
```

### Techniques: 

**TYPE** | **EXAMPLE** |
-------- | ----------- |
Direct file inclusion | /etc/passwd |
Current directory (number of . varies depeding on web app directory) | .. |
Bypassing filters | ....// |
URL encoding | Double encoding |

### PHP Filter: 

The `PHP` filter wrapper is used in `LFI` to read the actual `PHP` page content. In typical cases, it is not possible to read a `PHP` files content via `LFI` because `PHP` files get executed and never show the existing code. However, we can use the `PHP` filter to display the content of `PHP` files in other encoding formats such as `Base64` or `ROT13`.

```
http://example.thm.labs/page.php?file=php://filter/resource=/etc/passwd
```

We now simply need to convert the `Base64` or `ROT13` back to plain text, we can do that with [`ROT13` Decoder](https://onlinecryptotools.com/rot13-decrypt-data) or [`Base64` Decoder](https://onlinebase64tools.com/base64-decode). 

### PHP Data: 

The PHP data wrapper is used to include raw plain text or `Base64` encoded data. It is also used to include images on the current page. 

```
http://example.thm.labs/page.php?file=data://text/plain;base64,QW9DMyBpcyBmdW4hCg==
```

As a result the page will show our original plain text message. Using this technique we can also include `PHP` code into a page, by encoding the required code and including it into the `PHP` data wrapper. 

### LFI to RCE via Log file:

The `LFI` to `RCE` via `PHP` log file is also called a log poisoning attack, it is used to gain remote command execution on the webserver.
The attacker needs to include a malicious payload into services log files such as `Apache`, `SSH`, etc.
Then the `LFI` vulnerability is used to request the page that includes the malicious payload. 

### Terminal$$

```
$ curl -A "<?php phpinfo();?>" http://LAB_WEB_URL.p.thmlabs.com/login.php
```

### LFI to RCE via PHP Session:

The `LFI` to `RCE` via `PHP` session follows the same concept as log poisoning.
`PHP` session are files withign the operating system that store temporary information. 

This technique requries to read the `PHP` configuratio fiel first and then we know where the `PHP` session files are.
Then we include `PHP` code into the session and finally call the file via `LFI`. 

`PHP` stores session data in files within the system in different locations based on the configuration, following locations are some of the most common locations that `PHP` stores: 

```
c:\Windows\Temp 
/tmp/ 
/var/lib/php5 
/var/lib/php/session
```

To find the `PHP` session file name, `PHP` uses the following naming scheme by default `sess_<SESSION_ID>`, where we can find the `SESSION-ID` using the browser and verifying cookies sent from the browser. 

So we open the developer tools and then the **Application** tab and add the value from the `PHPSESSID` to our `sess_` to get the filename.
With this and the location found previously we know now where the file is located.

![Session](https://raw.githubusercontent.com/MathewHDYT/OneShare/main/_images/session.png)

```
https://LAB_WEB_URL.p.thmlabs.com/login.php?err=/tmp/sess_vc4567al6pq7usm2cufmilkm45
```

### Tools:

`CMD` can be used to create curl requests to a webserver.
