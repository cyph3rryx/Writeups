# login   →  100Pts.

1. We land on the site with a username and password page (login page)
2. Tried Username and Password...nothing was there. (no bypass)
3. Fuzzed the files and found out there is a file named index.js which had a Incorrect Password hash.
4. Copied the hashed string and decoded to Base64 and got the flag.

(Note: I used Burp to decode the string)
