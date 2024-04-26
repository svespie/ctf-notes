Tags: #picoCTF-2021 #netcat #nc #ascii #python

https://play.picoctf.org/practice/challenge/156

![](../../../../_attachments/Pasted%20image%2020240425201222.png)

This requires you to interact with a remote system via a shell. 

Hopefully it's still live. Also, it may have been updated by the time you are reading and want to try this. Definitely navigate to the challenge URL and confirm.

`mercury.picoctf.net 22902`

![](../../../../_attachments/Pasted%20image%2020240425201502.png)

Well, that's interesting. These look like decimal values that could represent characters. A quick review of the ASCII table suggests that these values might be decimal encodings of ASCII characters.

[ASCII table - Table of ASCII codes, characters and symbols (ascii-code.com)](https://www.ascii-code.com/)

Instead of translating this by hand, I opted to generate a script to help. This type of task is common in CTF challenges.

``` python
filepath = "input.txt"

result = ""

with open(filepath, "r") as f:
    for line in f:
        char = chr(int(line.strip()))
        result += char

print(result)
```

This code simply takes in an input file and for each line convert the (decimal) value into a character. All of the characters are grouped together, in order they appear in the file, into a single string. That final string is displayed before the script exits.

This requires capturing the output from the netcat command into a file that the script know the path to. The following command is an example.

``` bash
$ nc mercury.picoctf.net 22902 > input.txt
```

When the script is executed, we can see if the output is as we expected.

![](../../../../_attachments/Pasted%20image%2020240425203106.png)

If this didn't work out on the first try, we would step back and evaluate our original hypothesis.

