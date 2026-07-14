<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/cross-site-request-forgery.svg"></p>

An application does not check whether incoming requests come from a trusted or intended source. A threat actor can exploit this vulnerability by creating a malicious request and deceiving an authenticated user into interacting with it, such as by clicking a link or visiting a harmful webpage. Since the victim's browser automatically includes their valid session cookies, the application may process the unauthorized request as if it were initiated by the legitimate user. This could potentially lead to unauthorized actions being carried out on the victim's account.

Clone this current repo recursively
```sh
git clone --recurse-submodules https://github.com/qeeqbox/cross-site-request-forgery
```
Run the webapp using Python
```sh
python3 cross-site-request-forgery/vulnerable-web-app/webapp.py
```
Open the webapp in your browser 127.0.0.1:5142
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/1.png"></p>
Use Jane's default credentials (username: jane and password: jane) to login
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/2.png"></p>
Run the payload.html, this payload will change Jane's password 
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/4.png"></p>
Log out
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/4.png"></p>
Use Jane's default credentials (username: jane and password: jane) to login
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/5.png"></p>
Jane is no longer able to log in, the password has been changed
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/6.png"></p>

## Code
When a user sends a POST request to change their password, the change_password() function will be called
```py
...
...
elif parsed_url.path == "/change-password" and "password" in post_request_data:
    self.send_content(200, [('Content-type', 'text/html')], self.change_password(post_request_data["password"][0]))
    return
...
...
```
The change_password() does not verify whether the request is intended or not by the user
```py
def change_password(self, password):
    with connect(DATABASE, isolation_level=None, check_same_thread=False) as connection:
        cursor = connection.cursor()
        cursor.execute("UPDATE users SET hash='%s' WHERE username='%s'" % (sha512(password.encode()+SALT).hexdigest(),self.session["username"]))
```

