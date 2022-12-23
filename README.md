<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/cross-site-request-forgery.png"></p>

A threat actor may trick an authenticated or trusted victim into transmitting unauthorized actions on their behalf.

## Example #1
1. Threat actor crafts an exploit URL for a fund transfer from a vulnerable website
2. bob logs in to the vulnerable website (bob is authenticated - session cookie is saved)
3. Threat actor tricks bob into clicking on the exploit URL
4. bob clicks on the exploit URL
5. bob's browser loads the session cookie and performs a fund transfer

## Example #2
1. Threat actor crafts an exploit for a fund transfer from a vulnerable website
2. Threat actor uploads the exploit to a malicious website
3. bob logs in to the vulnerable website (bob is authenticated - session cookie is saved)
4. Threat actor tricks bob into clicking on the malicious website
5. bob clicks on the malicious website
6. bob's browser loads the session cookie and performs a fund transfer

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
