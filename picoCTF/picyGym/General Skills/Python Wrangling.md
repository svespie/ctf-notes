Tags: #picoCTF-2021 #python

https://play.picoctf.org/practice/challenge/166

![](../../../_attachments/Pasted%20image%2020240425184526.png)

We are presented with a python script, a password file containing a string of characters, and an encrypted flag.txt file.

![](../../../_attachments/Pasted%20image%2020240425185129.png)

This python script appears to be able to encrypt and decrypt a file. The `-d` option decrypts a file. When looking at the code for decryption, we can see that an undocumented optional argument containing the decryption password is possible.

![](../../../_attachments/Pasted%20image%2020240425185633.png)

The rest of the code appears to be correct. We'll give it the college try.

![](../../../_attachments/Pasted%20image%2020240425185724.png)

We could just as easily have only passed in the decrypted file path and let the script do its job.

![](../../../_attachments/Pasted%20image%2020240425185920.png)

Challenges like this are an introduction to learning and getting comfortable with code (python code in this case).