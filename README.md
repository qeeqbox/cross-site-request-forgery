<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/cross-site-request-forgery.png"></p>

A threat actor may trick an authenticated or trusted victim into transmitting unauthorized actions on their behalf.

## Example #1
1. Threat actor crafts an exploit URL for a fund transfer from a vulnerable website
2. Bob logs in to the vulnerable website (Bob is authenticated - session cookie is saved)
3. Threat actor tricks Bob into clicking on the exploit URL
4. Bob clicks on the exploit URL
5. Bob's browser loads the session cookie and performs a fund transfer

## Example #2
1. Threat actor crafts an exploit for a fund transfer from a vulnerable website
2. Threat actor uploads the exploit to a malicious website
3. Bob logs in to the vulnerable website (Bob is authenticated - session cookie is saved)
4. Threat actor tricks Bob into clicking on the malicious website
5. Bob clicks on the malicious website
6. Bob's browser loads the session cookie and performs a fund transfer

## Code
#### Target Logic
```py
...
@app.route("/send_money", methods=["POST"])
@login_required
def send_money():
   ...
   amount = int(request.form.get("amount"))
   to_user = get_user(int(reques.form.get("to")))
   if current_user:
      if current_user['balance'] <= amount:
         current_user['balance'] -= amount
         to_user['balance'] += amount
         return make_response({"transfer":"success"}, 200)
   return make_response({"transfer":"failed"}, 200)
...
```

#### Victim Executes
```html
<html>
  <body>
    <form action="https://test.local/send_money" method="POST">
      <input type="hidden" amount="1000" to="9999"/>
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

## Impact
Vary

## Risk
- Read & modify data
- Execute commands

## Redemption
- Header verification
- Challenge-response
- Anti-csrf tokens
- Same-site cookies

## Names
 - XSRF
 - Sea Surf
 - Session Riding
 - Cross-Site reference forgery
 - Hostile linking

## ID
776dd60c-c1de-46a3-a104-25cd836e24b6

## References
- [wiki](https://en.wikipedia.org/wiki/cross-site_request_forgery)
