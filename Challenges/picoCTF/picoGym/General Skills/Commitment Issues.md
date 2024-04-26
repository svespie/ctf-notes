Tags: #picoCTF-2024 #git 

https://play.picoctf.org/practice/challenge/411

![](../../../../_attachments/Pasted%20image%2020240425215834.png)

We are provided a zip file. When we unzip it, we can see that this is a git repository.

![](../../../../_attachments/Pasted%20image%2020240425220054.png)

Fortunately, we can search local commit logs for strings:

`git log -p | grep -i picoCTF`

![](../../../../_attachments/Pasted%20image%2020240425220402.png)



