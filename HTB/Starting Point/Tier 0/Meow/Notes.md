2024-03-27 | 12:06

Tags: #htb #startingpoint #very-easy #telnet #recon #weak_credentials #misconfiguration



| IP                   |                                  |
| -------------------- | -------------------------------- |
| Domain / Workgroup   |                                  |
| Hostname             | Meow                             |
| Host OS              | Linux 5.4.0-77-generic           |
| Host Architecture    | x86_64                           |
| Open Services        | TCP 23 (telnet)                  |
| Vulnerabilities      | weak / default credentials       |
| User Accounts/Creds  | root:$blank$                     |
| Admin Accounts/Creds |                                  |
| User Flag            | b40abdfe23665f766f9c61ecba8a4c19 |
| Root Flag            | N/A                              |

### Summary
The only TCP 23 appears to be open. Upon connection is a login page. Brute forcing / lucky guessing yields access to the system.

For some reason, I could not get hydra to work. There is a notable delay in the server response. Perhaps that was intentional to thwart brute force attempts?

### Lessons Learned
Wordlists are important. Even more important, is getting the damn tool to work correctly.

### Notes
The walkthrough solved this with some simple guessing:
* admin:
* administrator:
* root:

### Narrative
Default port scan:

`$ sudo nmap -n --open --reason 10.129.96.145`

![[../../../../_attachments/Pasted image 20240327122045.png]]

Connecting to the telnet service provided a login interface after some time.

$ telnet 10.129.96.145

![[../../../../_attachments/Pasted image 20240327122450.png]]

Tried a few basic passwords. No dice.

Deeper port scan found only TCP 23 open (UDP not checked yet). Service scan on TCP 23:

`$ sudo nmap -n -p 23 -sCV 10.129.96.145`

![[../../../../_attachments/Pasted image 20240327123115.png]]

With no much else to go on, a list of common telnet credentials was identified from SecLists:

[SecLists/Passwords/Default-Credentials/telnet-betterdefaultpasslist.txt at master · danielmiessler/SecLists (github.com)](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/telnet-betterdefaultpasslist.txt)

I put all of the telnet-* default creds file into a single wordlist.

![](../../../../_attachments/Pasted%20image%2020240327220802.png)

Hydra...

`hydra -C creds.lst 10.129.168.164 telnet`

![](../../../../_attachments/Pasted%20image%2020240327220958.png)



![](../../../../_attachments/Pasted%20image%2020240327223044.png)

Setting creds2.lst to the three credentials in the walkthrough, tested the response. It seems to work OK.

![](../../../../_attachments/Pasted%20image%2020240327223701.png)

Trying the bigger list again, this was an interesting result:

![](../../../../_attachments/Pasted%20image%2020240327224004.png)

I suspect that it may have been moving too quickly. When performing this manually, once "root" is entered for the login the server grants access right away without need for a password.