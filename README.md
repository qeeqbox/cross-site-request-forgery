<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/cross-site-request-forgery.svg"></p>

## Cross-Site Request Forgery (CSRF)
Cross-Site Request Forgery (CSRF) is a web security vulnerability that allows an attacker to trick an authenticated user's browser into sending unauthorized requests to a trusted web application. Because browsers automatically include authentication credentials, such as session cookies, the vulnerable application may process these requests as if the authenticated user intended to perform them.

Unlike Cross-Site Scripting (XSS), which involves injecting malicious scripts that execute in a user's browser, CSRF exploits the user's existing authenticated session to perform unwanted actions on their behalf.

## How CSRF Works
1. User Authentication: The user logs into a trusted web application and receives an authentication mechanism, such as a session cookie.
2. Attacker Creates a Malicious Request: The attacker creates a malicious website, email, or link containing a request targeting the trusted application. This request could change account settings, submit a form, or transfer funds.
3. Victim Visits the Malicious Content: When the authenticated user visits the attacker's website or interacts with the malicious content, their browser sends the forged request to the trusted application.
4. Server Processes the Request: The browser automatically includes the user's authentication cookies with the request. If the application lacks proper CSRF protections, the server may accept the request as legitimate and execute the requested action.

## Impact of CSRF
Successful CSRF exploitation can enable attackers to perform unauthorized actions, such as:
- Changing account settings
- Updating email addresses or passwords
- Making financial transactions
- Submitting unauthorized forms
- Deleting or modifying user data
CSRF typically does not allow attackers to directly read sensitive information from the victim's account, as browsers enforce the Same-Origin Policy. Instead, attackers usually manipulate the application state by executing unauthorized actions.

## CSRF Mitigation Strategies
To prevent CSRF vulnerabilities:
1. Anti-CSRF Tokens: The most common defense is to require a unique, unpredictable token with every state-changing request. The server verifies that the submitted token matches the one stored in the user's session before processing the request.
2. SameSite Cookies: Configure cookies with the SameSite attribute to limit the ability of cross-site requests to include authentication cookies. Common settings include:
   - Strict: Cookies are only sent in same-site requests.
   - Lax: Cookies are restricted for most cross-site requests while allowing normal navigation.
   - None: Cookies can be sent cross-site but require Secure.
3. Validate Request Origin: Applications can check the Origin or Referer headers to confirm that requests come from trusted sources. This header validation should be used as an additional measure since headers may not always be available or reliable.
4. Require Additional Verification for Sensitive Actions: For high-risk operations, request further confirmation, such as:
   - Password re-entry
   - Multi-factor authentication (MFA)
   - Transaction confirmation
5. Use Secure Cookie Settings: Setting cookie security configurations enhances overall session protection:
   - HttpOnly: Prevents JavaScript from accessing session cookies.
   - Secure: Ensures cookies are transmitted only over HTTPS.

## CSRF Example

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
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/content/3.png"></p>
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

