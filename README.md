<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/cross-site-request-forgery/main/cross-site-request-forgery.png"></p>

An adversary may trick an authenticated or trusted victim into transmitting unauthorized actions on their behalf.

## Example #1
1. adversary crafts an exploit URL for a fund transfer from a vulnerable website
2. bob logs in to the vulnerable website (bob is authenticated - session cookie is saved)
3. adversary tricks bob into clicking on the exploit URL
4. bob clicks on the exploit URL
5. bob's browser loads the session cookie and performs a fund transfer

## Example #2
1. adversary crafts an exploit for a fund transfer from a vulnerable website
2. adversary uploads the exploit to a malicious website
3. bob logs in to the vulnerable website (bob is authenticated - session cookie is saved)
4. adversary tricks bob into clicking on the malicious website
5. bob clicks on the malicious website
6. bob's browser loads the session cookie and performs a fund transfer

## Impact
Vary

## Risk
- read & modify data
- execute commands

## Redemption
- header verification
- challenge-response
- anti-csrf tokens
- same-site cookies

## Names
 - xsrf
 - sea surf
 - session riding
 - cross-site reference forgery
 - hostile linking

## ID
776dd60c-c1de-46a3-a104-25cd836e24b6

## References
- [wiki](https://en.wikipedia.org/wiki/cross-site_request_forgery)
