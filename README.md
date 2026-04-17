
# Custom Web Application Firewall (WAF)

This project is a functional **Web Application Firewall (WAF)** built with Python. I created this while studying Ethical Hacking to better understand how to defend web applications against common attacks at the **Application Layer (Layer 7)**.

Instead of just blocking IP addresses, this WAF performs **Deep Packet Inspection (DPI)**. It looks inside every request to find and stop malicious code before it reaches the server.



## How It Works
The WAF is built using a **Middleware Architecture**. This means it acts as a gatekeeper. Every time a browser sends a request, the WAF intercepts it, runs a series of security checks, and only "allows" the request if it is 100% safe.

### Key Security Features
* **Deep Packet Inspection (DPI):** Scans the URL, Headers, and Body for malicious patterns.
* **Attack Signature Detection:** Uses Regular Expressions (Regex) to block:
    * **SQL Injection (SQLi):** e.g., `UNION SELECT`, `' OR 1=1 --`
    * **Cross-Site Scripting (XSS):** e.g., `<script>`, `alert()`
    * **Directory Traversal:** e.g., `../../etc/passwd`
    * **Command Injection:** e.g., `; whoami`, `| ls`
* **Anti-Smuggling Defense:** Detects and blocks **HTTP Request Smuggling** by identifying conflicting `Content-Length` and `Transfer-Encoding` headers.
* **Host Validation:** Only allows traffic meant for specific, whitelisted domains to prevent **Host Header Injection**.
* **Bot Protection:** Blocks common hacking tools like `sqlmap`, `nmap`, and `nikto` based on their User-Agent.
* **Payload Decoding:** Automatically decodes **URL-encoded** data so that hackers cannot hide scripts behind special characters (like `%3Cscript%3E`).

---

## Tech Stack
* **Language:** Python 3.x
* **Framework:** Flask (for middleware and routing)
* **Tools:** `urllib` (for decoding), `re` (for pattern matching)

---

## Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/chinthajayanth/python-waf.git
    cd python-waf
    ```

2.  **Install Flask:**
    ```bash
    pip install Flask
    ```

3.  **Run the WAF:**
    ```bash
    python waf_app.py
    ```

The WAF will start running on `http://localhost:8080`.

---

## Testing the Defense
You can test the WAF using `curl` in your terminal to see it block real attacks.

### 1. Test SQL Injection (Blocked)
```bash
curl -X POST http://localhost:8080/ -d "username=admin' OR 1=1 --"
```

### 2. Test XSS in URL (Blocked)
```bash
curl "http://localhost:8080/search?q=<script>alert(1)</script>"
```

### 3. Test Host Header Spoofing (Blocked)
```bash
curl -H "Host: evil-site.com" http://localhost:8080/
```

### 4. Test Malicious Bot Detection (Blocked)
```bash
curl -A "sqlmap/1.5.8" http://localhost:8080/
```

---

## What I Learned
Building this project helped me understand:
1.  **HTTP Anatomy:** The precise structure of headers, blank lines, and bodies.
2.  **Evasion Techniques:** How hackers use encoding to bypass simple firewalls.
3.  **Defensive Coding:** Why "Deny by Default" is the safest way to build security software.
4.  **Protocol Desync:** The dangers of how different servers interpret the same request.

---

## Disclaimer
This project is for **educational purposes only**. While it is a great way to learn about web security, it should not be used as the only line of defense for a high-traffic production application. Always follow the **Defense in Depth** principle.

---

