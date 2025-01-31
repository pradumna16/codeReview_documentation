# Learning Web Penetration Testing through Code Review (Beginner's Guide)

---

## 1. Introduction to Web Penetration Testing and Code Review

### What is Web Penetration Testing?

Web penetration testing is the process of identifying and exploiting vulnerabilities in web applications to improve security.

### What is Code Review?

Code review is the practice of analyzing source code to find security flaws before attackers exploit them.

### Why Code Review Matters

- Identifies vulnerabilities early in development.
- Complements dynamic testing (e.g., scanning tools).
- Helps understand the root cause of security issues.

---

## 2. Prerequisites

### Basic Knowledge

To effectively perform code review, you should have basic knowledge of:

- **Web Technologies**: HTML, JavaScript, HTTP/S, REST APIs.
- **Server-Side Languages**: PHP, Python, Node.js.
- **Databases**: SQL, NoSQL.
- **Common Vulnerabilities**: [OWASP Top 10](https://owasp.org/www-project-top-ten/).

### Tools to Install

- **Code Editors**: [VS Code](https://code.visualstudio.com/), [Sublime](https://www.sublimetext.com/).
- **Static Analysis Tools**: [Bandit](https://bandit.readthedocs.io/) (Python), [SonarQube](https://www.sonarqube.org/).
- **Web Proxies**: [Burp Suite](https://portswigger.net/burp), [OWASP ZAP](https://www.zaproxy.org/).

---

## 3. Core Concepts for Code Review

### Common Web Vulnerabilities

1. **Injection Attacks (SQLi, Command Injection)**\
   Look for unsanitized user input passed to queries or system commands.

   ```php
   // Bad: SQL Injection
   $query = "SELECT * FROM users WHERE id = " . $_GET['id'];
   ```

2. **Cross-Site Scripting (XSS)**\
   Check if user input is rendered without escaping.

   ```javascript
   // Bad: Reflected XSS
   document.write('<%= userInput %>');
   ```

3. **Broken Authentication**\
   Look for hardcoded credentials, weak session tokens, or missing MFA.

   ```python
   # Bad: Hardcoded password
   if user_password == "admin123":
       grant_access()
   ```

4. **Sensitive Data Exposure**\
   Avoid plaintext passwords, unencrypted data, or exposed API keys.

   ```javascript
   // Bad: Exposed API key
   const API_KEY = "sk_live_123456";
   ```

5. **Security Misconfigurations**\
   Look for default settings, verbose error messages, or open cloud storage.

   ```python
   # Bad: Debug mode enabled in production
   DEBUG = True
   ```

---

## 4. Setting Up a Code Review Environment

### Local Lab Setup

- Use Docker to containerize vulnerable apps (e.g., [OWASP Juice Shop](https://owasp-juice.shop/)).
- Clone open-source projects from GitHub to practice.

### Version Control Basics

```bash
git clone https://github.com/example/insecure-app.git
```

### IDE Plugins for Security

- **JavaScript**: ESLint, CodeQL (VS Code).
- **Python**: Bandit.
- **PHP**: PHPStan.

---

## 5. Step-by-Step Code Review Guide

### Step 1: Authentication and Authorization

Check for password complexity, insecure session tokens, and missing RBAC.

```php
// Bad: No rate-limiting
if ($_POST['password'] == $stored_password) {
    login_user();
}
```

### Step 2: Input Validation

Search for unsanitized inputs.

```python
# Bad: SQL Injection
query = "SELECT * FROM users WHERE name = '%s'" % request.GET['name']
```

### Step 3: Database Interactions

Identify dynamic queries or NoSQL injection.

```javascript
// Bad: No input sanitization
db.users.find({ username: req.body.username });
```

### Step 4: File Handling

Check for path traversal.

```php
// Bad: Unsafe file read
readfile("/var/www/html/" . $_GET['file']);
```

### Step 5: Security Headers

Ensure headers like CSP and HSTS are set.

```nginx
# Good: Secure headers
add_header Content-Security-Policy "default-src 'self'";
```

---

## 6. Practical Examples

### Example 1: SQL Injection Fix (PHP)

**Vulnerable Code**:

```php
$id = $_GET['id'];
$result = mysqli_query($conn, "SELECT * FROM users WHERE id = $id");
```

**Fixed Code**:

```php
$stmt = $conn->prepare("SELECT * FROM users WHERE id = ?");
$stmt->bind_param("i", $id);
```

### Example 2: XSS Fix (JavaScript)

**Vulnerable Code**:

```javascript
document.getElementById("output").innerHTML = userInput;
```

**Fixed Code**:

```javascript
document.getElementById("output").textContent = userInput;
```

---

## 7. Tools for Automated Code Review

| Static Analysis     | Dynamic Analysis |
| ------------------- | ---------------- |
| Bandit (Python)     | Burp Suite       |
| ESLint (JavaScript) | OWASP ZAP        |
| Brakeman (Ruby)     |                  |

---

## 8. Real-World Case Studies

1. **OWASP Juice Shop**: Practice on a deliberately insecure app.
2. **WordPress Plugins**: Analyze popular plugins for XSS/SQLi.

---

## 9. Best Practices

- Use **parameterized queries** and **input validation libraries**.
- Follow the **principle of least privilege**.
- Regularly audit dependencies (`npm audit`, `pip check`).

---

## 10. Additional Resources

### Recommended Books

- *The Web Application Hacker’s Handbook*
- *Secure Coding in PHP*

### Labs & Training

- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [Hack The Box](https://www.hackthebox.com/)

---

## 11. Conclusion

- Code review uncovers hidden vulnerabilities.
- Start small, use tools, and practice consistently.

---

## Appendices

### Glossary

- **SAST**: Static Application Security Testing.
- **CSP**: Content Security Policy.

### Cheat Sheet

- **Risky Functions**:

```regex
(eval|exec|system|passthru|shell_exec)\(
```

---

📌 **Note**: Clone this repo and contribute improvements!

**--Yours Truly HangMan**

