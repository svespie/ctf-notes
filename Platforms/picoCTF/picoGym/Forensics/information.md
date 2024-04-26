Tags: #picoCTF-2021 #forensics #exiftool #metadata

https://play.picoctf.org/practice/challenge/186

![](../../../../_attachments/Pasted%20image%2020240425192715.png)

Here we are provided a file that appears to be an image by name: cat.jpg.

Sometimes CTF makers try to be tricky. It is not a bad practice to run the `file` command any file you're given.

![](../../../../_attachments/Pasted%20image%2020240425193643.png)

When opening the file, it does in fact open as expected. Now is a good time to take a quick look at the content to see if perhaps the flag is written out in the image.

![](../../../../_attachments/Pasted%20image%2020240425193809.png)

An adorable cat to be sure, but we have work to do.

Looking for strings is another common tactic to find hidden text data.

![](../../../../_attachments/Pasted%20image%2020240425194444.png)

Nothing overly useful here. Many files contain metadata and JPG files are one file type that does.

![](../../../../_attachments/Pasted%20image%2020240425200027.png)

The license line looks interesting. It could be nothing, but some CTF challenges are more "game-like" than others. Challenges around images (especially steganography) tend to fall into this category.

It turns out that this is indeed encoded data containing the objective.

![](../../../../_attachments/Pasted%20image%2020240425200345.png)

Have a hard time with that answer? Me too. The popularity of this challenge is fairly low. Don't feel bad. Move on.