# More cookies  →  90 Pts.

1. We need an admin rights to view the cookie.

2. Inspected the page -> Applications -> Cookie was saved as "auth_name" and then the value: 

`a3c0ZTZtN1pRYTRKdkVmSXhKTit6bmdLWWgyaXYzZlZwcFZXQ3FWaE5QSzJKWTBBVVNRQ2FDS2d0Zk1USlUxa2M5djNnYnpwNEJjNitIZElLNU9mQXJyaForajRKdnhJYnhoOFRGSUFqbjYzZ0h4ZlEzUFBNcTU1Qks5WXlaUW0=`

3. We need to decode this then and only then we will get the flag.

4. The algorithm used here is CBC and according to that we need to find. (googled the answer as I didn't knew how to handle cookies with admin privilege)

5. Found a script on GitHub for decoding the code.

``` python
import requests
import base64

s=requests.Session()
s.get('[http://mercury.picoctf.net:10868/](http://mercury.picoctf.net:10868/)')
cookie = s.cookies['auth_name']
unb64 = base64.b64decode(cookie)
unb64b = base64.b64decode(unb64)

// first 128-bit block is IV, flip IV bit to flip plaintext bit "admin=0" to "admin=1"

// looping through bytes (16 bytes because 128-bit block)

for i in range(0, 128):
pos=i//8
guessdec = unb64b[0:pos] + (unb64b[pos]^(1<<(i%8))).to_bytes(1, 'big') + unb64b[pos+1:]
guess=base64.b64encode(base64.b64encode(guessdec))
r=requests.get('[http://mercury.picoctf.net:10868/](http://mercury.picoctf.net:10868/)', cookies={"auth_name": guess.decode()})
if "pico" in r.text:
print("Flag: " + r.text.split("<code>")[1].split("</code>")[0])
break

```

6. After running this script we will get our Flag in the output.

Note: What this script does is that it gets the cookies info from the provided site, decode the cookie to base64 and then uses the concept of CBC to flip the IV bit and loop it.
