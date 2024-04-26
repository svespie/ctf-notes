Tags: #picoCTF-2021 

https://play.picoctf.org/practice/challenge/163

![](../../../../_attachments/Pasted%20image%2020240425204252.png)

We are given an unknown file and a bash script. Always take what the challenge is giving you. Here is the bash script:

``` bash
#!/bin/bash

echo "Attempting disassembly of $1 ..."

#This usage of "objdump" disassembles all (-D) of the first file given by 
#invoker, but only prints out the ".text" section (-j .text) (only section
#that matters in almost any compiled program...

objdump -Dj .text $1 > $1.ltdis.x86_64.txt

#Check that $1.ltdis.x86_64.txt is non-empty
#Continue if it is, otherwise print error and eject

if [ -s "$1.ltdis.x86_64.txt" ]
then
	echo "Disassembly successful! Available at: $1.ltdis.x86_64.txt"

	echo "Ripping strings from binary with file offsets..."
	strings -a -t x $1 > $1.ltdis.strings.txt
	echo "Any strings found in $1 have been written to $1.ltdis.strings.txt with file offset"

else
	echo "Disassembly failed!"
	echo "Usage: ltdis.sh <program-file>"
	echo "Bye!"
fi

```

We can infer from this script that `noise` is a binary. We can confirm that.

![](../../../../_attachments/Pasted%20image%2020240425204812.png)

No surprise but we are building muscle memory. A quick check for strings:
![](../../../../_attachments/Pasted%20image%2020240425205024.png)

The bash script takes a different approach, attempting to decompile the binary and then storing any strings off. This may be a more complete approach, but often times the simpler path is sufficient - especially when first interacting with an unknown file.

![](../../../../_attachments/Pasted%20image%2020240425205248.png)

This challenge is introducing us to disassembly techniques we can begin to use for future exercises. This script is probably one we can put in our toolbelt for future use.


