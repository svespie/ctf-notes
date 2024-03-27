2024-03-27 | 12:06

Tags: #htb #startingpoint #very-easy #telnet #recon #weak_credentials #misconfiguration



| IP                   |       |
| -------------------- | ----- |
| Domain / Workgroup   |       |
| Hostname             |       |
| Host OS              | Linux |
| Host Architecture    |       |
| Open Services        |       |
| Vulnerabilities      |       |
| User Accounts/Creds  |       |
| Admin Accounts/Creds |       |
| User Flag            |       |
| Root Flag            |       |

### Summary



### Lessons Learned



### Notes



### Narrative
Default port scan:

`$ sudo nmap -n --open --reason 10.129.96.145`

![[attachments/Pasted image 20240327122045.png]]

Connecting to the telnet service provided a login interface after some time.

$ telnet 10.129.96.145

![[attachments/Pasted image 20240327122450.png]]

Tried a few basic passwords. No dice.

Deeper port scan found only TCP 23 open (UDP not checked yet). Service scan on TCP 23:

`$ sudo nmap -n -p 23 -sCV 10.129.96.145`

![[attachments/Pasted image 20240327123115.png]]

