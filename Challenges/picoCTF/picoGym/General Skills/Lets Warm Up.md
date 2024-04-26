Tags: #picoCTF-2019 #encoding #ascii 

https://play.picoctf.org/practice/challenge/22

![](../../../../_attachments/Pasted%20image%2020240425214142.png)

PicoCTF does not penalize for hints and the challenge description is not clear, particularly when considering flag format.

For this, review the ASCII table, which often contains different encoding formats.

[ASCII table - Table of ASCII codes, characters and symbols (ascii-code.com)](https://www.ascii-code.com/)

![](../../../../_attachments/Pasted%20image%2020240425214342.png)

0x70 represents the ASCII value 'p'. (case matters)

Taking the information from the hint, we simply put this value in as the flag.

`picCTF{p}`

