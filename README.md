# Wireshark Lab: How Websites Work (HTTP GET)

In this lab, I used Wireshark to trace a full HTTP GET request and response between my virtual machine and the website `neverssl.com`. The goal was to understand how web traffic works—from the DNS query and TCP handshake to the HTML payload being sent back by the server. To avoid browser redirects to the HTTPS version of the site, I had to use PowerShell as a “backdoor” method for sending a plain HTTP request that Wireshark could capture.

---

## 🔍 What I Did

1. **Started Wireshark** on my Windows VM to capture traffic.
2. **Used PowerShell** to send a manual HTTP GET request to `http://neverssl.com`.
3. Captured the **DNS Query** resolving the domain name to an IP address.
4. Tracked the **TCP 3-Way Handshake** establishing the connection.
5. Located the **HTTP GET request** in Wireshark.
6. Examined the **HTTP 200 OK response** from the server.
7. Reassembled and viewed the **payload** containing the HTML source.
8. Analyzed the full **HTML page** using Wireshark’s "Follow TCP Stream" view.

---

## 🖼️ Screenshots (Click to View Full Size)

1. ![Start Wireshark](./screenshots/1-start-wireshark-capture.png)
2. ![PowerShell HTTP Request](./screenshots/2-powershell-http-request.png)
3. ![DNS Query Neverssl](./screenshots/3-dns-query-neverssl.png)
4. ![TCP Handshake](./screenshots/4-tcp-handshake-neverssl.png)
5. ![HTTP GET Request](./screenshots/5-http-get-request.png)
6. ![HTTP Response](./screenshots/6-http-response.png)
7. ![HTTP Response Payload](./screenshots/7_http_response_payload.png)
8. ![HTML Response in TCP Stream](./screenshots/8-html-response-neverssl.png)

---

### 🧪 Scripts and Wireshark Filters Used

```powershell
# PowerShell Command to Perform HTTP GET Request (Unencrypted)
Invoke-WebRequest -Uri http://neverssl.com
```

```wireshark
# Wireshark Display Filter to Isolate DNS Query to neverssl.com
dns.qry.name == "neverssl.com"
```

```wireshark
# Wireshark Display Filter to Show TCP Handshake with Destination IP
ip.addr == 34.223.124.45 and tcp
```

```wireshark
# Wireshark Display Filter to Show HTTP Requests Only
http.request
```

```wireshark
# Wireshark Display Filter to Show HTTP Responses Only
http.response
```

```wireshark
# Wireshark Display Filter to Follow TCP Stream
tcp.stream eq 30
```
---

## 🧠 Why This Matters

This lab mirrors the kind of analysis I would do in a phishing investigation. For example, when a phishing email contains a shady URL, I can use this same technique to trace where the URL leads, what server it connects to, and what kind of response or payload it delivers—just like I did here. The PowerShell method helped me avoid browser security features that automatically redirect to HTTPS, letting me capture a raw, unencrypted HTTP transaction.

---

## ✅ Key Takeaways

- DNS translates the domain to an IP address before the connection begins.
- TCP uses a three-way handshake to establish communication.
- The HTTP GET method requests the web page from the server.
- The server’s response can be reassembled and reviewed line by line.
- This technique helps uncover redirect behavior and unexpected payloads—exactly what you'd look for in phishing or malware analysis.

---

**SEO Link:** `https://github.com/TheCyberFairy/how-websites-work-wireshark-lab`  
**Lab Description (≤350 characters):**  
I used Wireshark and PowerShell to bypass browser HTTPS redirects and analyze an HTTP GET request to `neverssl.com`. I followed the DNS query, TCP handshake, and full HTML response to simulate how phishing links can be examined in a security investigation.