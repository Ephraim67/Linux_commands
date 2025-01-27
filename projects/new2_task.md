### **Explaining Wireshark Filters**
In this step, you will learn how to use advanced display filters in Wireshark to perform more complex analysis tasks.
Wireshark filters provide a powerful way to narrow down the traffic you want to inspect. Here’s a breakdown of the filters you've mentioned and their functionality:

---

### **1. Displaying HTTP GET Requests on TCP Port 80**

**Filter:**
```bash
tcp.port == 80 and http.request.method == GET
```

**Explanation:**
- **`tcp.port == 80`**: This filter matches packets that are using TCP protocol and are directed to or from port 80. Port 80 is the default port for HTTP traffic.
- **`http.request.method == GET`**: This filter looks for HTTP GET requests specifically. The HTTP GET method is used to request data from a specified resource, typically when you visit a website (like `www.example.com`).
- **`and`**: This logical operator combines both conditions, meaning both must be true for the packet to match the filter.

So, the combined filter shows all the **HTTP GET requests** that are using **TCP port 80**, typically indicating regular web browsing traffic where the client (browser) is requesting resources like HTML files, images, or scripts from a web server.

---

### **2. Displaying TCP Packets with a Length Between 1000 and 2000 Bytes**

**Filter:**
```bash
tcp.len >= 1000 and tcp.len <= 2000
```

**Explanation:**
- **`tcp.len >= 1000`**: This part of the filter matches packets where the TCP payload (the data portion of the packet) is greater than or equal to 1000 bytes.
- **`tcp.len <= 2000`**: This part of the filter matches packets where the TCP payload is less than or equal to 2000 bytes.
- **`and`**: Again, the logical `and` operator means that both conditions must be true for the packet to match the filter.

This filter is useful for inspecting **TCP packets with a specific size range**, such as large data transfers like file uploads, downloads, or other sizable communications, where the data portion of the packet falls between 1000 and 2000 bytes in length.

---

### **3. Displaying HTTP Traffic to/from Google**

**Filter:**
```bash
http.host contains "google.com"
```

**Explanation:**
- **`http.host`**: This refers to the `Host` field in the HTTP request headers. The `Host` header specifies the domain name of the server (such as `google.com`) that the client wants to communicate with.
- **`contains "google.com"`**: This part of the filter checks whether the `http.host` field contains the string `google.com`. This means it will capture any HTTP traffic where the request or response involves the Google domain (e.g., `www.google.com`, `mail.google.com`, etc.).

This filter will show only the **HTTP traffic** involving **Google servers**. It's helpful when you want to isolate traffic to or from a particular website or service.

---

### **Combining Filters for Refined Analysis**

By combining filters, you can tailor the data to your exact needs. For example, you can filter **only Google HTTP traffic** that also involves **GET requests** on **TCP port 80** by combining all three filters like this:

```bash
tcp.port == 80 and http.request.method == GET and http.host contains "google.com"
```

This would show **HTTP GET requests** for **Google.com** specifically, on **port 80**, providing you with a very focused view of the traffic.

---

### **Conclusion**

Wireshark's display filters are incredibly useful for narrowing down and focusing on specific network traffic. By combining protocol, port, size, and application-layer filters, you can efficiently analyze network behavior, troubleshoot problems, and detect anomalies or security issues.
