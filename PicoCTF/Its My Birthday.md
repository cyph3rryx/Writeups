# Its My Birthday  →    100Pts.

Description:
I sent out 2 invitations to all of my friends for my birthday! I'll know if they get stolen because the two invites look similar, and they even have the same md5 hash, but they are slightly different! You wouldn't believe how long it took me to find a collision. Anyway, see if you're invited by submitting 2 PDFs to my website. [http://mercury.picoctf.net:11590/](http://mercury.picoctf.net:11590/)

1. So I boot up my burp and browser and loaded the website.
2. As per the description we have to upload 2 files which looks similar means they must have same name but different content. We need to check the collision of 2 files uploaded on the same server.
3. So I crafted 2 small PDF and saved them as hi.pdf in two different location in my PC and then uploaded them.
4. It reverted back the php code.
5. Was reading the code and found the flag there.
