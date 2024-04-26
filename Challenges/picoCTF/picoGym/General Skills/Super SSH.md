Tags: #picoCTF-2024 #php-reverse-shell 

https://play.picoctf.org/practice/challenge/424

![](../../../../_attachments/Pasted%20image%2020240425212635.png)

"Additional details will be available after launching your challenge instance."

![](../../../../_attachments/Pasted%20image%2020240425213015.png)

The key thing here is to simply connect. As another introduction challenge, this is to simply familiarize us with connecting to SSH services on non-standard TCP ports.

`$ ssh ctf-player@titan.picoctf.net -p 59002`

Note, the target host details may be different for you.

![](../../../../_attachments/Pasted%20image%2020240425213059.png)

On connection, the flag is provided and the connection terminated.