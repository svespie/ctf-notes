Tags: #picoCTF-2024 #git 

https://play.picoctf.org/practice/challenge/410

![](../../../../_attachments/Pasted%20image%2020240425222708.png)

This description is not great at determining where to find what we're looking for. The first hint talks about branching. Of course! Developers work collaboratively in branches so they don't step on each other.

`git branch -a`

This command lists all the branches. We can also see that we begin in main branch. Listing the contents of main, we see a single python file that is fairly generic.

![](../../../../_attachments/Pasted%20image%2020240425223445.png)

There are three feature branches. Let's have a look at each one.

![](../../../../_attachments/Pasted%20image%2020240425223559.png)

We see that each feature branch was addressing a different part of the flag. They can all be merged together into main for deployment. That operation is possible but unnecessary for this example. Copy paste will get us there.




