
Tags: #linux #htb #easy #joomla #CVE-2023-23752 #php-webshell #php-reverse-shell #sudo #apport-cli #CVE-2023-1326



| IP                   | 10.10.11.242                                                                                                            |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Domain / Workgroup   |                                                                                                                         |
| Hostname             | devvortex                                                                                                               |
| Host OS              | Linux, Ubuntu, 5.4.0-167-generic                                                                                        |
| Host Architecture    | x86_64                                                                                                                  |
| Open Services        | 22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9<br>80/tcp open  http    nginx 1.18.0 (Ubuntu)                      |
| Vulnerabilities      | Joomla permissions issues (CVE-2023-23752)<br>Sudo misconfiguration for logan user<br>Vulnerable apport (CVE-2023-1326) |
| User Accounts/Creds  | lewis:P4ntherg0t1n5r3c0n## (joomla admin, mysql)<br>logan:tequieromucho                                                 |
| Admin Accounts/Creds |                                                                                                                         |
| User Flag            | *redacted*                                                                                                              |
| Root Flag            | *redacted*                                                                                                              |

### Summary

This is an appropriately labeled "easy" machine that is more realistic than game-like. 

In typical CTF fashion, only TCP 80 and 22 are open. The initial web app was fruitless, but virtual host enumeration revealed a second web application at dev.devvortex.htb. A directory enumeration revealed a Joomla admin portal.

The Joomla admin portal led to the discovery of CVE-2023-23752, where there is a permissions issue on multiple files that potentially contain sensitive information. In this case, the initial set of credentials for a user called lewis were exposed to unauthenticated users via */administrator/manifests/files/joomla.xml*. These credentials allowed access to the administrator portal.

Initially, a webshell was published to the site as an extension/plugin. This became cumbersome and a more stable and interactive shell was required. Through the web shell, a PHP reverse shell was established and shortly thereafter upgraded via python3.

With an interactive shell, mysql was accessed using the same lewis credentials used to obtain admin access to the Joomla admin portal. From there, the users table was dumped and a bcrypt hash for the logan user was obtained. In typical CTF fashion, the rockyou wordlist was sufficient to crack this hash.

Once able to SSH into the machine as the logan user, it was discovered that this user was able to sudo a debugging application called apport-cli. It didn't take long to discover a privilege escalation CVE related to this application: CVE-2023-1326. After triggering a crash by killing the sleep command, the exploit steps were executed to obtain root access.

### Lessons Learned

- Enumeration continues to be an important activity, as this machine demonstrates.
- Be mindful of rabbit holes. Try 5 things for 5 minutes each and move on. Circle back, if needed.
- Use boxes like this to experiment with different techniques, time permitting.

### Notes

Turn off Windows defender before saving your plaintext notes locally. :)

### Narrative

Initial port scan.

![](../../_attachments/Pasted%20image%2020240422081903.png)

A more exhaustive TCP port scan was pretty much the same.

![](../../_attachments/Pasted%20image%2020240422081957.png)

Service scan on TCP 22 and TCP 80:

![](../../_attachments/Pasted%20image%2020240422082057.png)

This might be something to keep in mind but doesn't help us yet:

[CVE-2023–38408 (OpenSSH Vulnerability to RCE) | by Manjil | Medium](https://medium.com/@mane_csit2075/cve-2023-38408-openssh-vulnerability-to-rce-9e756a0369fd)

This may also be useful to keep in mind:

[NGINX through 1.18.0 allows an HTTP request smuggling... · CVE-2020-12440 · GitHub Advisory Database · GitHub](https://github.com/advisories/GHSA-6wvc-hc5h-7fqv)

The site isn't real interesting. Neither is a basic directory bust:

![](../../_attachments/Pasted%20image%2020240422111616.png)

A common tactic employed by CTF makers is to leverage virtual hosting. The scan is still running but we have something to check out while it finishes:

![](../../_attachments/Pasted%20image%2020240422122206.png)

Directory busting on this virtually hosted subdomain reveals something interesting:

![](../../_attachments/Pasted%20image%2020240422173524.png)

This leads to a Joomla admin login page. Searching for a possible bypass, led to this blog post identifying a potentially sensitive file exposed to unauthenticated users:

[CVE-2023-23752: Joomla Authentication Bypass Vulnerability (pingsafe.com)](https://www.pingsafe.com/blog/cve-2023-23752-joomla-authentication-bypass-vulnerability/)

![](../../_attachments/Pasted%20image%2020240422174057.png)

/administrator/manifests/files/joomla.xml

The version is 4.2.6. It seems that Joomla 4.2.6 is vulnerable to CVE-2023-23752:

[Joomla! CVE-2023-23752 to Code Execution - Blog - VulnCheck](https://vulncheck.com/blog/joomla-for-rce)

Inspired by this blog post, the following request was made against the API (unauthenticated):

![](../../_attachments/Pasted%20image%2020240422180750.png)

`lewis`
`P4ntherg0t1n5r3c0n##`

Unfortunately, these credentials did not allow SSH access. It did, however, allow access to the admin portal.

The admin portal allows content control, but the following code could not be inserted into into /templates/cassiopeia/index.php:

``` php
<?php if (isset($GET['cmd'])) system($_GET['cmd']); ?>
```

We can, however, install an extension/plugin. 

[GitHub - p0dalirius/Joomla-webshell-plugin:](https://github.com/p0dalirius/Joomla-webshell-plugin)

Now simple curl commands can be used to execute commands against the target.

![](../../_attachments/Pasted%20image%2020240422200635.png)

In obtaining the /etc/passwd file, we can see that the lone user is "logan".

![](../../_attachments/Pasted%20image%2020240422201003.png)

We discovered we can write to the root directory, where we planted a PHP reverse shell:

[php-reverse-shell/php-reverse-shell.php · GitHub](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)

Upgraded the shell:

``` bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

Logged into mysql:

![](../../_attachments/Pasted%20image%2020240422212529.png)

We found creds for a user called logan in sd4fg_users:

![](../../_attachments/Pasted%20image%2020240422212712.png)

`logan`
`$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12`

Cracked with John the Ripper:

![](../../_attachments/Pasted%20image%2020240422213124.png)

`logan`
`tequieromucho`

Successful SSH into the machine:

![](../../_attachments/Pasted%20image%2020240422213303.png)

User flag is found in logan's home directory.

![](../../_attachments/Pasted%20image%2020240422213447.png)

The logan user is able to run /usr/bin/apport-cli as sudo. 

![](../../_attachments/Pasted%20image%2020240422214014.png)

This looks promising:

[GitHub - diego-tella/CVE-2023-1326-PoC](https://github.com/diego-tella/CVE-2023-1326-PoC)

The basic premise is to trigger a crash dump and then exploit the vulnerability.

![](../../_attachments/Pasted%20image%2020240422220330.png)

Once control is relinquished to the user, enter the following:

``` bash
!/bin/bash
```

Boom!

![](../../_attachments/Pasted%20image%2020240422215915.png)



