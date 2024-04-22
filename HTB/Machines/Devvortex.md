{{date}} | {{time}}

Tags: #linux #htb #easy 



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

