# Day 9 XSS Game Challenge - Learning Journey

A walkthrough of the Google XSS Game challenges and key security concepts learned.

---

## Level 1: Hello, World of XSS
**URL:** https://xss-game.appspot.com/level1/frame

**Solution:**
```
query=<script>alert('xss')</script>
```

**What I Learned:**
- Basic XSS injection through URL parameters
- Direct script tag injection when input is reflected without sanitization
- The most straightforward form of reflected XSS

---

## Level 2: Persistence is Key
**URL:** https://xss-game.appspot.com/level2/frame

**Solution:**
```
post-content=<img src='foobar' onerror='alert("xss")'>
```

**What I Learned:**
- XSS doesn't always require `<script>` tags
- Event handlers like `onerror` can execute JavaScript
- Using HTML attributes to trigger code execution
- Stored/persistent XSS through post content

---

## Level 3: That Sinking Feeling...
**URL:** https://xss-game.appspot.com/level3/frame#1

**Solution:**
```
URL=https://xss-game.appspot.com/level3/frame#1' onerror='alert("xss")'>
```

**What I Learned:**
- Fragment identifiers (hash values) can be attack vectors
- Breaking out of HTML attribute context with quotes
- Injecting malicious attributes into existing tags
- Context-aware injection techniques

---

## Level 4: Context Matters
**URL:** https://xss-game.appspot.com/level4/frame

**Solution:**
```
timer=');alert('xss
```

**What I Learned:**
- JavaScript context injection (breaking out of function calls)
- Importance of understanding the execution context
- String concatenation vulnerabilities in JavaScript
- Syntax manipulation to inject code mid-execution

---

## Level 5: Breaking Protocol
**URL:** https://xss-game.appspot.com/level5/frame

**Solution:**
```
URL=https://xss-game.appspot.com/level5/frame/signup?next=javascript%3Aalert%28%27xss%27%29
```

**What I Learned:**
- `javascript:` protocol can execute code in href attributes
- URL encoding bypasses (`%3A` for `:`, `%28` for `(`, `%29` for `)`)
- Protocol-based XSS vectors
- Redirect parameter vulnerabilities

---

## Level 6: Follow the X
**URL:** https://xss-game.appspot.com/level6/frame#/static/gadget.js

**Solution:**
```
URL=https://xss-game.appspot.com/level6/frame#data:text/plain,alert('xss')
```

**What I Learned:**
- Data URIs as XSS vectors
- `data:` protocol exploitation
- Bypassing external resource restrictions
- Dynamic script loading vulnerabilities

---

## Key Takeaways

### Core XSS Concepts
1. **Input Validation is Critical** - Never trust user input
2. **Context Awareness** - XSS vectors differ based on where input is reflected (HTML, JavaScript, attributes, URLs)
3. **Multiple Attack Vectors** - XSS isn't just `<script>` tags:
   - Event handlers (`onerror`, `onload`, etc.)
   - Protocol handlers (`javascript:`, `data:`)
   - HTML attribute injection
   - JavaScript context breaking

### Defense Strategies
- Proper output encoding based on context
- Content Security Policy (CSP)
- Input sanitization and validation
- Avoiding dangerous sinks (innerHTML, eval, etc.)
- URL validation and allowlisting

### Encoding Techniques Learned
- URL encoding for special characters
- HTML entity encoding
- JavaScript string escaping
- Context-specific encoding requirements

---

## Conclusion

Completing these challenges demonstrated that XSS vulnerabilities can manifest in many forms and contexts. The key to both exploiting and defending against XSS is understanding:
- Where user input flows in the application
- What context it's rendered in
- How to properly encode/escape for that specific context

**Remember:** Well that wasn't so tough... Was it? 😉