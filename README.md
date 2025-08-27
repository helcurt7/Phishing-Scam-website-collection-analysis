# Phishing-demostartion
Recently i modified a blackeye.sh with the aid of gpts since i was not profficient in those  (phishing tool by EricksonAtHome) for demonstarting an phising site i will just have some pictures and how we can avoid from phising attacks where our password credentials from unauthorized users(due to legal issues i not able to provide the source code)

<img width="1723" height="935" alt="image" src="https://github.com/user-attachments/assets/0ca0a9ef-fe3b-4db6-9aff-cc69049f6bf6" />


As u can see left screen is my script and right side is the phishing website , as an attacker i will send the link
[*] Send this link to the victim: https://thy-u-mar-rid.trycloudflare.com generated from my script to the victim via email or any form of social media, as the victim press the link it will show up a fake web page (example amazon here)  baiting u log in and capture your credential.
(but the fake website shd be hosted until the victim key in into the website)
once it capture the fake web will be terminated and store the credential into a file and victim web page will be redirect to the real amazon websites.

how do we diffrentiate the phishing website 
1. check the link isit the real amazon login page
lets compate it the fake one:

 https://thy-u-mar-rid.trycloudflare.com


the real one : 

https://www.amazon.com/ap/signin?openid.pape.max_auth_age=0&openid.return_to=https%3A%2F%2Fwww.amazon.com%2F%3Fref_%3Dnav_signin&openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.assoc_handle=usflex&openid.mode=checkid_setup&openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0

notice there is amazon.com not saofjaso.com 

2. when we usually used website does not need to log in please think before enter any credential especially password.


