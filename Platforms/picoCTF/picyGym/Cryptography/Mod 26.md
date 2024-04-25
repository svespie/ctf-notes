Tags: #picoCTF-2021 #cryptography

[https://play.picoctf.org/practice/challenge/144](https://play.picoctf.org/practice/challenge/144)

![](../../../../_attachments/Pasted%20image%2020240425183635.png)

`cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_GYpXOHqX}`

The answer is in the question, but we can see that the first letter is c and that the entire "ciphertext" is generally in the format we would expect the flag to be in (right down to the capitalization).

cvpbPGS => picoCTF

Comparing c to p, we notice that the letters are 13 places apart in the alphabet:

![](../../../../_attachments/Pasted%20image%2020240425184003.png)

Here is a little bit about the ROT13 cipher: [ROT13 - Wikipedia](https://en.wikipedia.org/wiki/ROT13).

Cyberchef makes quick to reverse this cipher. In a 26 character alphabet, we simply need to apply ROT13 again.

![](../../../../_attachments/Pasted%20image%2020240425184240.png)

