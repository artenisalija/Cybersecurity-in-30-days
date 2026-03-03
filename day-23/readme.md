# Day 23 Web Application Security

## Accessing Web Applications

The first thing I learned is that all you need to access a web application is a **browser**. It's the gateway to interacting with any web-based service, which also means it's the primary surface through which vulnerabilities can be exploited.

## Identifying Security Risks

During this module, I got hands-on experience spotting real security weaknesses in a login page.

The first thing I noticed was that the login page had **no limit on login attempts** — I could try as many username/password combinations as I wanted without the app slowing me down or locking the account. This falls under the **Identification and Authentication Failure** category. It's a serious risk because it leaves the door wide open for brute-force attacks.

The second issue I caught was that the username and password were being transmitted in **cleartext** — completely unencrypted. That means anyone intercepting the traffic could read the credentials directly. This is classified as a **Cryptographic Failure**, one of the most critical web security risks because it exposes sensitive data in transit.

## Practical Lab: Exploiting IDOR

In the final task, I worked through a real-world scenario involving **Insecure Direct Object Reference (IDOR)**. By examining other user accounts in the application, I was able to identify which account had been used to make malicious changes. After reverting those changes, I captured the flag:

```
THM{IDOR_EXPLORED}
```

This exercise taught me how IDOR vulnerabilities allow attackers to access or modify data belonging to other users simply by manipulating object references — no special privileges required.