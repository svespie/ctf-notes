[BabyEncryption](https://app.hackthebox.com/challenges/BabyEncryption)

CHALLENGE DESCRIPTION

*You are after an organised crime group which is responsible for the illegal weapon market in your country. As a secret agent, you have infiltrated the group enough to be included in meetings with clients. During the last negotiation, you found one of the confidential messages for the customer. It contains crucial information about the delivery. Do you think you can decrypt it?*

![BabyEncryption.zip](../../../../_attachments/BabyEncryption.zip)

PW: hackthebox

The zip file contains two files:

1. chall.py
2. msg.enc

chall.py

``` python
import string
from secret import MSG

def encryption(msg):
    ct = []
    for char in msg:
        ct.append((123 * char + 18) % 256)
    return bytes(ct)

ct = encryption(MSG)
f = open('./msg.enc','w')
f.write(ct.hex())
f.close()
```

This straight forward script encrypts a message and writes it to file as a hex string.

![](../../../../_attachments/Pasted%20image%2020240419181615.png)
The basic strategy is to basically reverse the encryption. This is possible because it is not very expensive computationally.

``` python
with open("msg.enc", "r") as f:
    ciphertext_hex = f.read()

ciphertext_bytes = bytes.fromhex(ciphertext_hex)
message = []
for char in ciphertext_bytes:
    for c in range(256):
        encrypted_value = (123 * c + 18) % 256
        if encrypted_value == char:
            message.append(c)

message_bytes = bytes(message)

print(message_bytes.decode())
```
![](../../../../_attachments/Pasted%20image%2020240419200039.png)