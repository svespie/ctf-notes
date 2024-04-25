![](../../../_attachments/Pasted%20image%2020240422225609.png)

Tags: #linux 

| IP                   | 10.10.11.252 |
| -------------------- | ------------ |
| Domain / Workgroup   |              |
| Hostname             |              |
| Host OS              | Linux,       |
| Host Architecture    |              |
| Open Services        |              |
| Vulnerabilities      |              |
| User Accounts/Creds  |              |
| Admin Accounts/Creds |              |
| User Flag            |              |
| Root Flag            |              |

### Summary



### Lessons Learned



### Notes



### Narrative

Initial port scan:

`$ sudo nmap -n --open -O --osscan-guess -oN initial.nmap 10.10.11.252`

![](../../../_attachments/Pasted%20image%2020240422230008.png)


Expanded TCP port scan.

`$ sudo nmap -n -p0- --open -Pn -oN tcp-ports.nmap 10.10.11.252` 

![](../../../_attachments/Pasted%20image%2020240422230219.png)

Service scan:

`$ sudo nmap -n -p22,80,443,33081 -sTV -oN tcp-services.nmap 10.10.11.252`

![](../../../_attachments/Pasted%20image%2020240422230421.png)

TCP 33081 is interesting.

