2024-03-27 | 22:44

Tags: #htb #startingpoint #ftp 



| IP                   |                                  |
| -------------------- | -------------------------------- |
| Domain / Workgroup   |                                  |
| Hostname             |                                  |
| Host OS              |                                  |
| Host Architecture    |                                  |
| Open Services        | TCP 21 FTP                       |
| Vulnerabilities      | anonymous login allowed          |
| User Accounts/Creds  |                                  |
| Admin Accounts/Creds |                                  |
| User Flag            | 035db21c881520061c53e0536e44f815 |
| Root Flag            |                                  |

### Summary
The only service identified was FTP on TCP 21. The version information did not lead to a useful known vulnerability. Further enumeration identified anonymous login access, which revealed the flag.txt file.

### Lessons Learned



### Notes



### Narrative
Initial port scan:

`$ sudo nmap -n --open --reason 10.129.195.114`

![](attachments/Pasted%20image%2020240327224912.png)

Service scan on TCP 21:

`$ sudo nmap -n -p 21 -sTV 10.129.195.114`

![](attachments/Pasted%20image%2020240327225255.png)

A quick look for an exploit for vsftpd 3.0.3 revealed that a remote DOS appears to be the only thing of note.

Attempted anonymous connection with nmap:

$ sudo nmap -n -p 21 -sTV --script "ftp-anon and safe" 10.129.195.114

![](attachments/Pasted%20image%2020240327225728.png)

It looks like we can anonymously reach the flag. Sweet.

![](attachments/Pasted%20image%2020240327225855.png)
