<img width="1337" height="802" alt="image" src="https://github.com/user-attachments/assets/612f1a89-f175-46e0-9383-3ddddd502ae4" />


<img width="1500" height="790" alt="image" src="https://github.com/user-attachments/assets/537b99df-fe99-4a25-84b4-c34dbe578331" />




Target URL: https://tng-malaysia.regisster.co
Analysis Environment: Kali Linux, Burp Suite Professional
Primary Objective: Analyze the web application for vulnerabilities, identify backend API logic, and determine the nature of the threat.

1. Executive Summary
The target tng-malaysia.regisster.co is a malicious phishing website masquerading as the legitimate Touch 'n Go (TNG) eWallet platform. It is not a standard credential-harvesting site, but rather a sophisticated Telegram Account Takeover (ATO) proxy. The scammers utilize fake TNG branding to trick victims into inputting their phone numbers and OTPs, which are then used in real-time to hijack the victim's Telegram messaging account.

2. Threat Intelligence & Infrastructure
Domain Trust: regisster.co has a low trust score and is flagged for phishing.

Hosting/WAF: The site is protected by Cloudflare (evident by Server: cloudflare headers and /cdn-cgi/rum telemetry endpoints).

Backend Architecture: PHP-based backend (index.php), interacting with a MySQL database (based on PHPSESSID and JSON responses).

Frontend: Utilizes jQuery and Bootstrap, heavily mimicking the TNG mobile UI.

3. The "Kill Chain" (How the Scam Works)
Through API enumeration and source code analysis, the following attack flow was identified:

The Lure: The victim visits the fake TNG site and enters their phone number.

The Telegram Proxy: The site calls the method=sendCode API. The scammers' backend triggers a real Telegram API request. Telegram sends a real OTP to the victim's Telegram app.

The Interception: The victim enters the OTP into the fake website. The API method=sendOtp captures this code and immediately forwards it to Telegram.

The 2FA Harvest (Phase 2): If the victim has Telegram Two-Step Verification (2FA) enabled, the scammers' backend receives a "passwordNeeded" response. The fake website then reloads and asks the victim for their "TNG Password" (which is actually their Telegram 2FA password).

4. Technical Findings & Vulnerabilities Identified
A. Broken Authentication / Lack of Rate Limiting (High Severity)

Endpoint: POST /my/home/API/index.php

Parameter: method=sendOtp&otp=...

Finding: The endpoint does not enforce any rate limiting, CAPTCHA, or account lockout mechanisms. It is highly vulnerable to automated brute-force attacks (using Burp Intruder) to guess the 5-digit OTP.

B. Client-Side Response Manipulation (Informational)

Technique: Intercepting the server's JSON response ("status":"failed") and altering it to "status":"success".

Finding: The attack was successfully intercepted via Burp Proxy, but it did not bypass authentication. This proves the backend enforces strict server-side session validation (PHPSESSID) and does not blindly trust client-side JSON responses.

C. Information Disclosure & Poor OpSec (Medium Severity)

Finding: Analysis of the HTML source code revealed a hardcoded phone number (60103555428) on the OTP page. This indicates the scammers are using a static phishing template and failed to properly randomize victim data, exposing their infrastructure's sloppiness.

Finding: The frontend JavaScript explicitly checks for data.result.detail == "passwordNeeded", clearly exposing the backend logic used to trigger the 2FA credential harvest phase.

5. Burp Suite Methodology Used
Proxy: Intercepted HTTP/2 traffic to map the application flow and capture AJAX requests.

Repeater: Sent crafted POST requests to test API behavior, test for parameter pollution, and verify SQLi (using ' and ").

Intruder: Prepared for brute-force testing of the sendOtp endpoint (00000-99999). (Note: Attack aborted to avoid triggering 100,000 SMS/callbacks to a live number).

Match & Replace: Configured automatic response rewriting to test for client-side authentication bypasses.

6. Conclusion & Recommendations
This exercise successfully reverse-engineered a live Telegram phishing kit. The site operates by proxying real OTPs and relies on social engineering (fake TNG UI) to harvest Telegram credentials.

Actionable Advice: Do not input any real credentials. If you entered a real phone number, immediately revoke all active Telegram sessions via the official Telegram app (Settings > Devices) and ensure Two-Step Verification is enabled with a strong password. Report the URL to Cloudflare and Google Safe Browsing.

💡 Tips for your Presentation:
Show the screenshots: Show the Burp Suite Repeater window where you sent the method=getStatus request and got the "failed" JSON back.

Show the Source Code: Point to the exact line in the HTML where it says 60103555428 and the JS that checks for "passwordNeeded". This is the "wow" factor that proves you understand the scam better than the scammers.

Emphasize Safety: Mention that you used a controlled environment (Kali VM) and did not actually complete the brute-force attack to avoid unintended side effects. This shows ethical hacking maturity.

This response is AI-generated and for reference purposes only.
