Tags: #python 

https://play.picoctf.org/practice/challenge/238

![](../../../../_attachments/Pasted%20image%2020240425232047.png)

We are provided two files. One is a python script we are to execute and the other is a text file that is expected to be in the same directory as the python script.

The contents of codebook.txt:

![](../../../../_attachments/Pasted%20image%2020240425232317.png)

The python code is as follows:

``` python
import random
import sys

def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])

flag_enc = chr(0x13) + chr(0x01) + chr(0x17) + chr(0x07) + chr(0x2c) + chr(0x3a) + chr(0x2f) + chr(0x1a) + chr(0x0d) + chr(0x53) + chr(0x0c) + chr(0x47) + chr(0x0a) + chr(0x5f) + chr(0x5e) + chr(0x02) + chr(0x3e) + chr(0x5a) + chr(0x56) + chr(0x5d) + chr(0x45) + chr(0x5d) + chr(0x58) + chr(0x31) + chr(0x5e) + chr(0x05) + chr(0x5f) + chr(0x53) + chr(0x5a) + chr(0x10) + chr(0x5f) + chr(0x0e) + chr(0x13)

def print_flag():
  try:
    codebook = open('codebook.txt', 'r').read()
    password = codebook[4] + codebook[14] + codebook[13] + codebook[14] +\
               codebook[23]+ codebook[25] + codebook[16] + codebook[0]  +\
               codebook[25]
    flag = str_xor(flag_enc, password)
    print(flag)
  except FileNotFoundError:
    print('Couldn\'t find codebook.txt. Did you download that file into the same directory as this script?')

def main():
  print_flag()

if __name__ == "__main__":
  main()
```

Just executing the python script decrypts the string of text stored in cookbook.txt. 

![](../../../../_attachments/Pasted%20image%2020240425232623.png)

This script is good to keep handy for future use, as it is a great demonstration of using python to XOR data.