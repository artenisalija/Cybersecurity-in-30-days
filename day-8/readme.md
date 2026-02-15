# Day 8 Natas Levels 6-10 Walkthrough

This guide covers solutions and explanations for Natas levels 6 through 10 from OverTheWire's Natas wargame series.

## Level 6 → Level 7

**Concept:** File Inclusion / Source Code Analysis

**Challenge:** The page has an input field asking for a secret. Viewing the source code reveals a PHP include statement that references an external file containing the secret.

**Solution:**
1. View the page source code
2. Notice the include statement: `include "includes/secret.inc"`
3. Navigate directly to `/includes/secret.inc` to view the secret
4. Enter the secret into the input field to get the password for natas7

**Key Learning:** Always check for included files that might be accessible directly through the web server.

---

## Level 7 → Level 8

**Concept:** Path Traversal / Local File Inclusion (LFI)

**Challenge:** The page has a simple navigation with Home and About links. The URL structure uses a parameter like `?page=home`.

**Solution:**
1. Notice the URL pattern uses a `page` parameter
2. The hint mentions that passwords are stored in `/etc/natas_webpass/natas8`
3. Modify the URL parameter to: `?page=/etc/natas_webpass/natas8`
4. The page will include and display the password file content

**Key Learning:** Path traversal vulnerabilities occur when user input is used to construct file paths without proper validation.

---

## Level 8 → Level 9

**Concept:** Reversing Encoded Data

**Challenge:** Similar to level 6, there's an input field for a secret, but this time the secret is encoded in the source code.

**Solution:**
1. View the page source code
2. Find the encoding function that processes the secret
3. The secret is typically: base64 encoded, reversed, and hex encoded (or similar)
4. Reverse the process:
   - Take the encoded secret from the source
   - Decode from hex to binary
   - Reverse the string
   - Decode from base64
5. Enter the decoded secret to get the password

**Example reversal in PHP:**
```php
$encodedSecret = "3d3d516343746d4d6d6c315669563362";
$secret = base64_decode(strrev(hex2bin($encodedSecret)));
```

**Key Learning:** Understanding encoding mechanisms allows you to reverse them to reveal original data.

---

## Level 9 → Level 10

**Concept:** Command Injection

**Challenge:** The page allows you to search for words in a dictionary file using grep.

**Solution:**
1. The input is passed directly to a shell command
2. Inject additional commands using shell metacharacters
3. Use the semicolon (`;`) or pipe (`|`) to chain commands
4. Example payload: `; cat /etc/natas_webpass/natas10 #`
   - The semicolon ends the grep command
   - `cat` displays the password file
   - The hash (`#`) comments out the rest of the original command

**Alternative payload:** `. /etc/natas_webpass/natas10`

**Key Learning:** Never trust user input that gets passed to system commands. Always sanitize and validate.

---

## Level 10 → Level 11

**Concept:** Filtered Command Injection

**Challenge:** Similar to level 9, but with some characters filtered out (like `;`, `|`, `&`).

**Solution:**
1. The filtering blocks common command injection characters
2. However, grep itself can accept multiple files as arguments
3. Craft input that uses grep's legitimate functionality
4. Example payload: `. /etc/natas_webpass/natas11`
   - The dot (`.`) matches any character in regex
   - This tells grep to search for any character in the password file
   - Since every line has at least one character, all lines are returned

**Alternative:** Use other regex patterns like `.*` or `^` that will match content

**Key Learning:** Even with filtering, think about how the legitimate tool functionality can be abused. Blacklist filtering is often insufficient.

---

## General Tips

- **Always view source code** - Many challenges hide important information in HTML comments or JavaScript
- **Check network requests** - Use browser developer tools to inspect requests and responses
- **Test input validation** - Try various special characters and payloads
- **Read error messages carefully** - They often reveal useful information about the system
- **Think like an attacker** - Consider what the application does with your input

## Resources

- [OverTheWire Natas](http://overthewire.org/wargames/natas/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Command Injection Guide](https://owasp.org/www-community/attacks/Command_Injection)

---

**Note:** This guide is for educational purposes. Only practice on authorized systems like OverTheWire's wargames.