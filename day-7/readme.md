# Day 7: Web Application Attack Surface Mapping

## 🎯 Objective
Map the complete attack surface of 2 web applications, discover hidden endpoints, and document interesting security findings.

---

## 📋 Prerequisites

### Required Tools
- [ ] **Burp Suite Community Edition** - [Download Here](https://portswigger.net/burp/communitydownload)
- [ ] **Web Browser** (Chrome/Firefox)
- [ ] **PortSwigger Academy Account** - [Sign Up Free](https://portswigger.net/web-security)
- [ ] **Text Editor** for documentation

### Time Required
- **Setup:** 15 minutes
- **Lab 1:** 1-2 hours
- **Lab 2:** 1-2 hours
- **Documentation:** 30 minutes
- **Total:** ~4-5 hours

---

## 🚀 Step-by-Step Guide

### Phase 1: Environment Setup (15 minutes)

#### 1.1 Install Burp Suite
```bash
# Download from: https://portswigger.net/burp/communitydownload
# Install based on your OS
# Launch Burp Suite Community Edition
```

#### 1.2 Configure Burp Suite
1. Launch Burp Suite
2. Create temporary project (default settings)
3. Go to **Proxy** → **Options**
4. Verify proxy listener is running on `127.0.0.1:8080`
5. Go to **Proxy** → **Intercept** → Turn **Intercept OFF** (for now)

#### 1.3 Configure Browser
**Option A: Use Burp's Built-in Browser** (Recommended)
- Click **Proxy** → **Intercept** → **Open Browser**
- This automatically configures proxy settings

**Option B: Configure Your Browser Manually**
- Firefox: Settings → Network Settings → Manual proxy
- HTTP Proxy: `127.0.0.1`, Port: `8080`
- Check "Use this proxy for all protocols"

#### 1.4 Test Configuration
1. With Burp running and browser configured, visit: http://burpsuite
2. You should see the Burp Suite page
3. If successful, you're ready to proceed!

---

### Phase 2: Select Your Target Labs (10 minutes)

#### 2.1 Access PortSwigger Academy
1. Go to: https://portswigger.net/web-security
2. Log in to your account
3. Navigate to **All Labs**

#### 2.2 Choose 2 Labs (Suggested)

**Lab 1: SQL Injection** (Information Disclosure Focus)
- Name: "SQL injection vulnerability in WHERE clause allowing retrieval of hidden data"
- URL: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data
- Click **"ACCESS THE LAB"**
- Copy the unique lab URL

**Lab 2: Authentication** (Endpoint Discovery Focus)
- Name: "Username enumeration via different responses"
- URL: https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses
- Click **"ACCESS THE LAB"**
- Copy the unique lab URL

---

### Phase 3: Attack Surface Mapping - Lab 1 (1-2 hours)

#### 3.1 Initial Reconnaissance (15 minutes)

**Manual Exploration:**
1. Open lab URL in Burp browser
2. Browse the entire website naturally
3. Click every link, button, and menu item
4. Note visible features:
   - Navigation menu
   - Search functionality
   - Login/registration forms
   - Product listings
   - Any filter/sort options

**Check Common Files:**
```
Visit these URLs manually:
- /robots.txt
- /sitemap.xml
- /.git/config
- /admin
- /backup
- /test
```

#### 3.2 Burp Site Map Analysis (20 minutes)

1. **View Site Map:**
   - Go to **Target** → **Site map**
   - Expand the tree for your lab domain
   - You should see all pages Burp captured

2. **Review HTTP History:**
   - Go to **Proxy** → **HTTP history**
   - Sort by URL, Method, Status Code
   - Look for patterns

3. **Identify Endpoints:**
   - Right-click on domain → Filter
   - Look for:
     - GET requests (pages, APIs)
     - POST requests (forms, actions)
     - PUT/DELETE (API endpoints)
     - Query parameters (`?id=`, `?category=`)

#### 3.3 Automated Crawling (15 minutes)

1. **Configure Scan:**
   - Right-click on your lab domain in Site map
   - Select **"Scan"**
   - Choose **"Crawl"** only (not audit - too slow)
   - Click **"OK"**

2. **Monitor Progress:**
   - Go to **Dashboard** → **Tasks**
   - Wait for crawl to complete (3-5 minutes)
   - Review new discovered endpoints

#### 3.4 Endpoint Inventory (20 minutes)

**Document Every Endpoint Found:**

Create a table with:
- **Path** - Full URL path
- **Method** - GET, POST, etc.
- **Parameters** - Query strings, form fields
- **Purpose** - What it does
- **Response Codes** - 200, 302, 404, etc.

Example:
```
/                    | GET  | None              | Homepage          | 200
/login               | GET  | None              | Login form        | 200
/login               | POST | username,password | Auth handler      | 302
/products            | GET  | category          | Product listing   | 200
/product/details     | GET  | productId         | Product details   | 200
/my-account          | GET  | None              | User profile      | 302
```

#### 3.5 Interesting Findings (30 minutes)

**Analyze Each Endpoint:**

1. **Input Validation:**
   - Which parameters accept user input?
   - Are inputs reflected in responses?
   - Special characters filtered?

2. **Authentication/Authorization:**
   - Which pages require login?
   - Can you access admin areas?
   - Session cookies present?

3. **Error Messages:**
   - Trigger errors (invalid input, missing params)
   - Do errors reveal:
     - Database names?
     - File paths?
     - Software versions?

4. **Hidden Data:**
   - View page source for all pages
   - Look for:
     - HTML comments
     - Hidden form fields
     - JavaScript files
     - API endpoints in JS

5. **Technology Stack:**
   - Check HTTP headers in Burp
   - Look for `Server:`, `X-Powered-By:`
   - Cookie names (PHPSESSID, JSESSIONID)
   - Error messages (stack traces)

**Document Security Issues:**
```
FINDING #1: SQL Injection
- Location: /products?category=Gifts
- Payload: category=Gifts' OR 1=1--
- Impact: Reveals hidden products
- Evidence: [screenshot/response]

FINDING #2: Information Disclosure
- Location: Error pages
- Details: Database name revealed in error
- Evidence: [screenshot]
```

---

### Phase 4: Attack Surface Mapping - Lab 2 (1-2 hours)

**Repeat Phase 3 steps for Lab 2:**
- Follow sections 3.1 through 3.5
- Focus on authentication-specific endpoints
- Pay attention to login/logout flows
- Test username enumeration
- Document different responses

---

### Phase 5: Advanced Endpoint Discovery (30 minutes)

#### 5.1 JavaScript Analysis

**Extract URLs from JavaScript:**

1. In Burp Site map, find all `.js` files
2. Right-click each → **"Send to Repeater"**
3. In Repeater tab, send request
4. Search response for:
   - `/api/`
   - `endpoint`
   - `fetch("`
   - `XMLHttpRequest`
   - URL patterns

**Common Patterns to Search:**
```javascript
// API endpoints
fetch('/api/v1/users')
axios.get('/dashboard/stats')

// Hidden admin paths
if (isAdmin) { window.location = '/admin/panel'; }

// Hardcoded credentials (rare but happens)
const API_KEY = "sk_test_..."
```

#### 5.2 Parameter Discovery

**Find Hidden Parameters:**

1. **In Burp, use Intruder:**
   - Find a GET request with parameters
   - Right-click → **"Send to Intruder"**
   - Go to **Intruder** → **Positions**
   - Clear all, select the parameter value
   - Click **"Add §"**

2. **Load Parameter Wordlist:**
   - Go to **Payloads** tab
   - Load common parameter names:
     ```
     id
     user
     admin
     debug
     test
     key
     token
     api_key
     ```

3. **Start Attack:**
   - Click **"Start attack"**
   - Review responses for different status codes
   - Parameters that return different responses may be valid

---

### Phase 6: Documentation & Reporting (30 minutes)

#### 6.1 Create Attack Surface Report

**For Each Application, Document:**

```markdown
## Application 1: [Lab Name]

### Executive Summary
- Lab URL: [URL]
- Date Tested: [Date]
- Total Endpoints Found: [Number]
- Critical Findings: [Number]

### Technology Stack
- Web Server: [e.g., nginx, Apache]
- Backend Language: [e.g., PHP, Python, Java]
- Framework: [if detected]
- Database: [if detected]

### Attack Surface Inventory

#### Authenticated Endpoints
1. /my-account - User profile page
2. /logout - Session termination
[etc.]

#### Unauthenticated Endpoints
1. / - Homepage
2. /login - Authentication
3. /products - Product catalog
[etc.]

#### API Endpoints
1. /api/v1/products - GET - Product data
2. /api/v1/user/{id} - GET - User info
[etc.]

#### Hidden/Undocumented Endpoints
1. /debug - Debug console (found in JS)
2. /admin/backup - Backup functionality
[etc.]

### Input Validation Points
- Login form: username, password
- Search: query parameter
- Product filter: category parameter
- User profile: name, email, address
[Total: X input points]

### Interesting Findings

#### Finding 1: [Title]
- **Severity:** High/Medium/Low
- **Location:** /endpoint?param=value
- **Description:** Detailed description
- **Steps to Reproduce:**
  1. Step 1
  2. Step 2
  3. Step 3
- **Evidence:** [Screenshot or response data]
- **Impact:** What an attacker could do

#### Finding 2: [Title]
[Repeat structure]

### Risk Assessment
- High Risk Items: [Count]
- Medium Risk Items: [Count]
- Low Risk Items: [Count]
- Informational: [Count]
```

#### 6.2 Screenshot Evidence

**Capture These Screenshots:**
1. Burp Site Map showing full endpoint tree
2. HTTP History with interesting requests highlighted
3. Any SQL injection or vulnerability proof
4. Error messages with sensitive info
5. Hidden endpoints discovered

**How to Screenshot in Burp:**
- Right-click on request → **"Copy as curl command"** (for replication)
- Use OS screenshot tool for visual evidence

---

## 📊 Success Criteria

By end of day, you should have:

- [ ] Mapped 2 complete web applications
- [ ] Documented 10+ endpoints per application
- [ ] Identified technology stack for each app
- [ ] Found at least 3 interesting security findings
- [ ] Created comprehensive documentation
- [ ] Screenshots of evidence
- [ ] Practiced using Burp Suite effectively

---

## 🎓 Learning Outcomes

**Skills Gained:**
- Web application reconnaissance
- Burp Suite proficiency
- Endpoint discovery techniques
- Security documentation
- Vulnerability identification
- HTTP protocol understanding

---

## 📚 Additional Resources

### PortSwigger Resources
- **Web Security Academy:** https://portswigger.net/web-security
- **Burp Suite Documentation:** https://portswigger.net/burp/documentation
- **Cheat Sheets:** https://portswigger.net/web-security/cheat-sheet

### Alternative Practice Labs
- **OWASP WebGoat:** https://github.com/WebGoat/WebGoat
- **DVWA:** http://www.dvwa.co.uk/
- **HackTheBox Academy:** https://academy.hackthebox.com (free tier)
- **TryHackMe:** https://tryhackme.com (free rooms)

### Wordlists & Tools
- **SecLists:** https://github.com/danielmiessler/SecLists (parameter/directory lists)
- **OWASP ZAP:** https://www.zaproxy.org/ (alternative to Burp)

---

## 🔧 Troubleshooting

### Burp Suite Issues

**Problem:** Browser won't connect through Burp
- **Solution:** Check proxy settings (127.0.0.1:8080)
- **Solution:** Ensure Burp proxy listener is running
- **Solution:** Use Burp's built-in browser instead

**Problem:** HTTPS certificate errors
- **Solution:** Install Burp CA certificate
- **Solution:** Go to http://burpsuite → CA Certificate → Install

**Problem:** Burp is slow/freezing
- **Solution:** Disable unnecessary features (Scanner, Logger)
- **Solution:** Clear HTTP history regularly
- **Solution:** Increase Java heap size in settings

### Lab Access Issues

**Problem:** Lab URL expired
- **Solution:** Labs expire after a few hours - click "ACCESS THE LAB" again

**Problem:** Can't solve the lab
- **Solution:** Click "View solution" for hints
- **Solution:** Focus on mapping first, solving second

---

## 📝 Daily Report Template

```markdown
# Day 7 Report: Attack Surface Mapping

## Date: [Your Date]

## Applications Tested
1. [Lab 1 Name] - [URL]
2. [Lab 2 Name] - [URL]

## Time Spent
- Setup: [X hours]
- Lab 1: [X hours]
- Lab 2: [X hours]
- Documentation: [X hours]
- Total: [X hours]

## Endpoints Discovered
- Lab 1: [Number] endpoints
- Lab 2: [Number] endpoints

## Key Findings
1. [Finding 1]
2. [Finding 2]
3. [Finding 3]

## Challenges Faced
- [Challenge 1 and how you overcame it]
- [Challenge 2 and how you overcame it]

## What I Learned
- [Skill/concept 1]
- [Skill/concept 2]
- [Skill/concept 3]

## Next Steps
- [ ] Practice more complex labs
- [ ] Learn automated scanning tools
- [ ] Study specific vulnerability types
```

---

## 🎯 Next Steps (Day 8+)

After completing Day 7:
- **Day 8:** Deep dive into one vulnerability type (SQLi, XSS, etc.)
- **Day 9:** Automated scanning with Nikto, Nuclei
- **Day 10:** API security testing
- **Continue:** Complete more PortSwigger Academy labs

---

## ⚠️ Legal & Ethical Notice

**ONLY test on:**
- ✅ PortSwigger Academy labs
- ✅ Your own applications
- ✅ Platforms with explicit permission (HackTheBox, TryHackMe, etc.)

**NEVER test on:**
- ❌ Production websites without written permission
- ❌ Any site you don't own
- ❌ Company applications without authorization

*Unauthorized security testing is illegal and can result in criminal charges.*

---

## 💡 Pro Tips

1. **Take Breaks:** This is intensive work - take 10 min breaks every hour
2. **Document As You Go:** Don't wait until the end to document findings
3. **Screenshot Everything:** Visual evidence is crucial
4. **Read Responses:** Don't just look at status codes, read full responses
5. **Check Headers:** Security headers reveal a lot about the application
6. **Follow Redirects:** 302 redirects often hide interesting endpoints
7. **Test Edge Cases:** Empty params, special chars, very long inputs
8. **Save Your Work:** Export Burp state regularly (Project → Save copy)

---

**Good luck with Day 7! 🚀**

Remember: Security testing is a skill that improves with practice. Don't get discouraged if you don't find everything on your first try. The goal is learning the methodology.