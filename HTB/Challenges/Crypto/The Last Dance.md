[The Last Dance](https://app.hackthebox.com/challenges/The%2520Last%2520Dance)

CHALLENGE DESCRIPTION

To be accepted into the upper class of the Berford Empire, you had to attend the annual Cha-Cha Ball at the High Court. Little did you know that among the many aristocrats invited, you would find a burned enemy spy. Your goal quickly became to capture him, which you succeeded in doing after putting something in his drink. Many hours passed in your agency's interrogation room, and you eventually learned important information about the enemy agency's secret communications. Can you use what you learned to decrypt the rest of the messages?

![The Last Dance.zip](../../../_attachments/The%20Last%20Dance.zip)

pw: hackthebox

Two files:

1. out.txt
2. source.py


out.txt appears to contain an encrypted message:

![](../../../_attachments/Pasted%20image%2020240419144527.png)


source.py

``` python
from Crypto.Cipher import ChaCha20
from secret import FLAG
import os


def encryptMessage(message, key, nonce):
    cipher = ChaCha20.new(key=key, nonce=iv)
    ciphertext = cipher.encrypt(message)
    return ciphertext


def writeData(data):
    with open("out.txt", "w") as f:
        f.write(data)


if __name__ == "__main__":
    message = b"Our counter agencies have intercepted your messages and a lot "
    message += b"of your agent's identities have been exposed. In a matter of "
    message += b"days all of them will be captured"

    key, iv = os.urandom(32), os.urandom(12)

    encrypted_message = encryptMessage(message, key, iv)
    encrypted_flag = encryptMessage(FLAG, key, iv)

    data = iv.hex() + "\n" + encrypted_message.hex() + "\n" + encrypted_flag.hex()
    writeData(data)

```

After looking at the code, we know that the initialization vector (iv) is 'c4a66edfe80227b4fa24d431'. This is the value used as the nonce.

The second, longer line is the message. The last line is the encrypted flag.

The algorithm used is ChaCha20, which is a symmetric cipher. This means the same key is used to both encrypt and decrypt the message.

[ChaCha - Cryptography Primer (cryptography-primer.info)](https://www.cryptography-primer.info/algorithms/chacha/)
[ChaCha20 | libsodium](https://doc.libsodium.org/advanced/stream_ciphers/chacha20)

One thing to point out is that two messages are encrypted using the same key and the same nonce. While ChaCha20 does not appear to be vulnerable to a plaintext attack, it does appear to be vulnerable to a key reuse attack.

*Making use of the same key for encryption with a stream cipher without the use of a nonce can lead to exposure of confidential data. This is because in stream ciphers, the incoming plaintext is XORed with the cipher’s keystream to produce the corresponding ciphertext. If you have two ciphertexts encrypted with the same key, XORing these together will eliminate the keystream entirely, leaving you with the XOR of the original plaintexts.*

[Stream Cipher Key Reuse | Hacker101 (linuxsec.org)](https://hacker101.linuxsec.org/vulnerabilities/stream_reuse)

By XORing the original message with the encrypted message, they keystream can be resolved. Then this keystream can be XORed with the encrypted flag to obtain the plaintext flag.

The python code below demonstrates this.

``` python
original_message = b"Our counter agencies have intercepted your messages and a lot of your agent's identities have been exposed. In a matter of days all of them will be captured"

# read in the values of the output file
with open("out.txt", "r") as f:
    nonce = bytes.fromhex(f.readline().strip())
    encrypted_message = bytes.fromhex(f.readline().strip())
    encrypted_flag = bytes.fromhex(f.readline().strip())

# xor original message with encrypted message to obtain the keystream
keystream = bytearray()
for byte1, byte2 in zip(original_message, encrypted_message):
    keystream.append(byte1 ^ byte2)

# the encrypted flag originates from the same keystream as the original message, allowing a simple XOR to retrieve the plaintext
decrypted_flag = bytearray()
for byte1, byte2 in zip(keystream, encrypted_flag):
    decrypted_flag.append(byte1 ^ byte2)

print(decrypted_flag.decode())

```

