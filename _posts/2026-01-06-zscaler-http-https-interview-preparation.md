---
layout: post
title: "Zscaler Interview Preparation - HTTP/HTTPS Deep Dive"
date: 2026-01-06 12:00:00 -0000
categories: interview-preparation zscaler networking http https protocols
tags: zscaler http https networking protocols c-cpp linux interview web
excerpt: "Comprehensive guide to HTTP/HTTPS for Zscaler interviews covering protocol fundamentals, how it works, tools, C/C++ Linux libraries, and common interview questions."
---

# Zscaler Interview Preparation - HTTP/HTTPS Deep Dive

A comprehensive guide to HTTP and HTTPS protocols for Zscaler technical interviews, covering fundamentals, implementation details, practical tools, C/C++ Linux libraries, and common interview questions.

## 1. What is HTTP/HTTPS?

### HTTP (Hypertext Transfer Protocol)

**HTTP** is an application-layer protocol for transmitting hypermedia documents (HTML, JSON, XML, etc.) over the Internet. It's the foundation of data communication for the World Wide Web.

**Key Characteristics:**
- **Stateless**: Each request is independent
- **Request-Response**: Client sends request, server responds
- **Text-based**: Human-readable protocol
- **Port**: Default port 80

### HTTPS (HTTP Secure)

**HTTPS** is HTTP over TLS/SSL encryption, providing:
- **Encryption**: Data is encrypted in transit
- **Authentication**: Server identity verification
- **Integrity**: Data tampering detection
- **Port**: Default port 443

**Key Differences:**

| Feature | HTTP | HTTPS |
|---------|------|-------|
| Security | No encryption | Encrypted (TLS/SSL) |
| Port | 80 | 443 |
| Certificate | Not required | Required (SSL/TLS) |
| Performance | Faster | Slightly slower (overhead) |
| Use Case | Public content | Sensitive data, authentication |

---

## 2. How HTTP/HTTPS Works

### HTTP Request-Response Cycle

```
Client                                    Server
  |                                         |
  |-------- HTTP Request ---------------->|
  |    GET /index.html HTTP/1.1            |
  |    Host: example.com                   |
  |    User-Agent: Mozilla/5.0              |
  |                                         |
  |<------- HTTP Response ----------------|
  |    HTTP/1.1 200 OK                     |
  |    Content-Type: text/html             |
  |    Content-Length: 1234                |
  |                                         |
  |    <html>...</html>                    |
```

### HTTP Request Structure

```
Method SP Request-URI SP HTTP-Version CRLF
Header-Field: Value CRLF
Header-Field: Value CRLF
CRLF
[Message Body]
```

**Example HTTP Request:**

```http
GET /api/users HTTP/1.1
Host: api.example.com
User-Agent: Mozilla/5.0 (Linux; x86_64)
Accept: application/json
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
```

### HTTP Response Structure

```
HTTP-Version SP Status-Code SP Reason-Phrase CRLF
Header-Field: Value CRLF
Header-Field: Value CRLF
CRLF
[Message Body]
```

**Example HTTP Response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 123
Server: nginx/1.18.0
Date: Mon, 06 Jan 2026 12:00:00 GMT
Connection: keep-alive

{"users": [{"id": 1, "name": "John"}]}
```

### HTTP Methods

1. **GET**: Retrieve resource (idempotent, safe)
2. **POST**: Create resource or submit data
3. **PUT**: Update/replace resource (idempotent)
4. **PATCH**: Partial update
5. **DELETE**: Delete resource (idempotent)
6. **HEAD**: Get headers only (idempotent, safe)
7. **OPTIONS**: Get allowed methods
8. **TRACE**: Echo request (for debugging)
9. **CONNECT**: Establish tunnel (for proxies)

### HTTP Status Codes

**1xx Informational:**
- 100 Continue
- 101 Switching Protocols

**2xx Success:**
- 200 OK
- 201 Created
- 204 No Content
- 206 Partial Content

**3xx Redirection:**
- 301 Moved Permanently
- 302 Found (Temporary)
- 304 Not Modified
- 307 Temporary Redirect
- 308 Permanent Redirect

**4xx Client Error:**
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 405 Method Not Allowed
- 408 Request Timeout
- 429 Too Many Requests

**5xx Server Error:**
- 500 Internal Server Error
- 502 Bad Gateway
- 503 Service Unavailable
- 504 Gateway Timeout

### HTTP Headers

**Request Headers:**
- `Host`: Server domain name
- `User-Agent`: Client identifier
- `Accept`: Acceptable content types
- `Accept-Language`: Preferred languages
- `Accept-Encoding`: Compression support
- `Authorization`: Credentials
- `Cookie`: Stored cookies
- `Content-Type`: Request body type
- `Content-Length`: Request body size
- `Connection`: Connection control (keep-alive, close)
- `Cache-Control`: Caching directives

**Response Headers:**
- `Content-Type`: Response body type
- `Content-Length`: Response body size
- `Server`: Server software
- `Set-Cookie`: Cookie to set
- `Cache-Control`: Caching directives
- `Expires`: Expiration time
- `ETag`: Entity tag for caching
- `Location`: Redirect location
- `WWW-Authenticate`: Authentication challenge

### HTTP Versions

**HTTP/1.0:**
- One request per connection
- No persistent connections
- No pipelining

**HTTP/1.1:**
- Persistent connections (keep-alive)
- Pipelining (limited)
- Chunked transfer encoding
- Host header required
- Better caching

**HTTP/2:**
- Multiplexing (multiple streams)
- Header compression (HPACK)
- Server push
- Binary protocol
- Stream prioritization

**HTTP/3:**
- Based on QUIC (UDP)
- Built-in encryption
- Connection migration
- Reduced latency

### HTTPS Handshake Process

```
Client                                    Server
  |                                         |
  |-------- ClientHello ------------------>|
  |    TLS version, cipher suites           |
  |    Random, supported extensions         |
  |                                         |
  |<------- ServerHello -------------------|
  |    Selected TLS version, cipher suite   |
  |    Server certificate                   |
  |    ServerHelloDone                     |
  |                                         |
  |-------- ClientKeyExchange ------------>|
  |    Encrypted premaster secret           |
  |    ChangeCipherSpec                    |
  |    Finished (encrypted)                 |
  |                                         |
  |<------- ChangeCipherSpec --------------|
  |<------- Finished (encrypted) ---------|
  |                                         |
  |      Encrypted HTTP Communication       |
```

---

## 3. Tools to Use

### curl

**Purpose**: Command-line HTTP client

**Example Commands:**
```bash
# GET request
curl https://api.example.com/users

# POST request with JSON
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "John", "email": "john@example.com"}'

# Include headers
curl -v https://api.example.com/users

# Follow redirects
curl -L https://example.com

# Save cookies
curl -c cookies.txt https://example.com

# Use cookies
curl -b cookies.txt https://example.com

# Basic authentication
curl -u username:password https://api.example.com

# Custom headers
curl -H "Authorization: Bearer token" https://api.example.com

# Verbose output
curl -v https://api.example.com

# Show only headers
curl -I https://api.example.com

# Download file
curl -O https://example.com/file.txt

# HTTPS with certificate verification disabled (testing only)
curl -k https://api.example.com
```

### wget

**Purpose**: Command-line HTTP client (download-focused)

**Example Commands:**
```bash
# Download file
wget https://example.com/file.txt

# Download with output name
wget -O output.txt https://example.com/file.txt

# Continue partial download
wget -c https://example.com/file.txt

# Recursive download
wget -r https://example.com

# Limit rate
wget --limit-rate=200k https://example.com/file.txt

# Custom headers
wget --header="Authorization: Bearer token" https://api.example.com
```

### httpie

**Purpose**: User-friendly HTTP client

**Example Commands:**
```bash
# GET request
http GET https://api.example.com/users

# POST request
http POST https://api.example.com/users name=John email=john@example.com

# JSON request
http POST https://api.example.com/users \
  name=John \
  email=john@example.com

# Include headers
http GET https://api.example.com/users \
  Authorization:"Bearer token"
```

### Postman / Insomnia

**Purpose**: GUI HTTP client for API testing

**Features:**
- Request builder
- Response viewer
- Environment variables
- Collection management
- Automated testing

### Wireshark / tshark

**Purpose**: HTTP packet analysis

**Example Commands:**
```bash
# Capture HTTP traffic
sudo tshark -i eth0 -f "tcp port 80"

# Capture HTTPS traffic (TLS handshake visible)
sudo tshark -i eth0 -f "tcp port 443"

# Filter HTTP requests
tshark -r capture.pcap -Y "http.request"

# Filter HTTP responses
tshark -r capture.pcap -Y "http.response"

# Show HTTP headers
tshark -r capture.pcap -Y "http" -T fields -e http.request.uri
```

### Browser DevTools

**Purpose**: Built-in browser HTTP inspection

**Features:**
- Network tab: View all HTTP requests/responses
- Headers inspection
- Request/response body
- Timing information
- Performance analysis

### Apache Bench (ab)

**Purpose**: HTTP server benchmarking

**Example Commands:**
```bash
# Basic benchmark
ab -n 1000 -c 10 https://example.com/

# With authentication
ab -n 1000 -c 10 -A user:pass https://example.com/

# POST request
ab -n 1000 -c 10 -p data.json -T application/json https://api.example.com
```

### httperf

**Purpose**: HTTP performance testing

**Example Commands:**
```bash
# Basic test
httperf --server example.com --port 80 --uri / --num-conns 1000

# Rate limiting
httperf --server example.com --rate 100 --num-conns 1000
```

---

## 4. C/C++ Linux Libraries

### libcurl (C/C++)

**Most popular HTTP client library for C/C++**

#### Installation

```bash
# Ubuntu/Debian
sudo apt-get install libcurl4-openssl-dev

# Compile with
gcc -o program program.c -lcurl
```

#### Basic GET Request

```c
#include <stdio.h>
#include <curl/curl.h>

struct MemoryStruct {
    char *memory;
    size_t size;
};

static size_t WriteMemoryCallback(void *contents, size_t size, 
                                  size_t nmemb, void *userp) {
    size_t realsize = size * nmemb;
    struct MemoryStruct *mem = (struct MemoryStruct *)userp;
    
    char *ptr = realloc(mem->memory, mem->size + realsize + 1);
    if (!ptr) {
        return 0;
    }
    
    mem->memory = ptr;
    memcpy(&(mem->memory[mem->size]), contents, realsize);
    mem->size += realsize;
    mem->memory[mem->size] = 0;
    
    return realsize;
}

int main(void) {
    CURL *curl;
    CURLcode res;
    struct MemoryStruct chunk;
    
    chunk.memory = malloc(1);
    chunk.size = 0;
    
    curl = curl_easy_init();
    if (curl) {
        curl_easy_setopt(curl, CURLOPT_URL, "https://api.example.com/users");
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteMemoryCallback);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, (void *)&chunk);
        curl_easy_setopt(curl, CURLOPT_SSL_VERIFYPEER, 1L);
        curl_easy_setopt(curl, CURLOPT_SSL_VERIFYHOST, 2L);
        
        res = curl_easy_perform(curl);
        
        if (res != CURLE_OK) {
            fprintf(stderr, "curl_easy_perform() failed: %s\n",
                    curl_easy_strerror(res));
        } else {
            printf("%s\n", chunk.memory);
        }
        
        curl_easy_cleanup(curl);
        free(chunk.memory);
    }
    
    return 0;
}
```

#### POST Request with JSON

```c
#include <curl/curl.h>
#include <string.h>

int main(void) {
    CURL *curl;
    CURLcode res;
    
    struct curl_slist *headers = NULL;
    headers = curl_slist_append(headers, "Content-Type: application/json");
    
    const char *json_data = "{\"name\":\"John\",\"email\":\"john@example.com\"}";
    
    curl = curl_easy_init();
    if (curl) {
        curl_easy_setopt(curl, CURLOPT_URL, "https://api.example.com/users");
        curl_easy_setopt(curl, CURLOPT_POSTFIELDS, json_data);
        curl_easy_setopt(curl, CURLOPT_HTTPHEADER, headers);
        
        res = curl_easy_perform(curl);
        
        if (res != CURLE_OK) {
            fprintf(stderr, "curl_easy_perform() failed: %s\n",
                    curl_easy_strerror(res));
        }
        
        curl_slist_free_all(headers);
        curl_easy_cleanup(curl);
    }
    
    return 0;
}
```

#### Custom Headers and Authentication

```c
curl_easy_setopt(curl, CURLOPT_HTTPHEADER, headers);
curl_easy_setopt(curl, CURLOPT_USERPWD, "username:password");
curl_easy_setopt(curl, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);

// Bearer token
headers = curl_slist_append(headers, "Authorization: Bearer token123");
```

### libmicrohttpd (C)

**Lightweight HTTP server library**

#### Installation

```bash
sudo apt-get install libmicrohttpd-dev
gcc -o server server.c -lmicrohttpd
```

#### Simple HTTP Server

```c
#include <microhttpd.h>
#include <stdio.h>
#include <string.h>

#define PORT 8888

static int answer_to_connection(void *cls, struct MHD_Connection *connection,
                                const char *url, const char *method,
                                const char *version, const char *upload_data,
                                size_t *upload_data_size, void **con_cls) {
    const char *page = "<html><body>Hello from server!</body></html>";
    struct MHD_Response *response;
    int ret;
    
    response = MHD_create_response_from_buffer(strlen(page),
                                               (void *)page,
                                               MHD_RESPMEM_PERSISTENT);
    
    MHD_add_response_header(response, "Content-Type", "text/html");
    
    ret = MHD_queue_response(connection, MHD_HTTP_OK, response);
    MHD_destroy_response(response);
    
    return ret;
}

int main(void) {
    struct MHD_Daemon *daemon;
    
    daemon = MHD_start_daemon(MHD_USE_SELECT_INTERNALLY, PORT, NULL, NULL,
                              &answer_to_connection, NULL,
                              MHD_OPTION_END);
    
    if (NULL == daemon) {
        return 1;
    }
    
    getchar();
    
    MHD_stop_daemon(daemon);
    return 0;
}
```

### cpp-httplib (C++)

**Header-only HTTP library**

```cpp
#include "httplib.h"
#include <iostream>

int main(void) {
    // HTTP Server
    httplib::Server svr;
    
    svr.Get("/", [](const httplib::Request &req, httplib::Response &res) {
        res.set_content("Hello World!", "text/plain");
    });
    
    svr.Get("/users", [](const httplib::Request &req, httplib::Response &res) {
        res.set_content("[{\"id\":1,\"name\":\"John\"}]", "application/json");
    });
    
    svr.Post("/users", [](const httplib::Request &req, httplib::Response &res) {
        // Process POST data
        std::cout << "Body: " << req.body << std::endl;
        res.set_content("User created", "text/plain");
    });
    
    svr.listen("0.0.0.0", 8080);
    return 0;
}

// HTTP Client
void http_client_example() {
    httplib::Client cli("https://api.example.com");
    
    auto res = cli.Get("/users");
    if (res) {
        std::cout << res->status << std::endl;
        std::cout << res->body << std::endl;
    }
    
    // POST request
    httplib::Params params;
    params.emplace("name", "John");
    params.emplace("email", "john@example.com");
    
    auto post_res = cli.Post("/users", params);
}
```

### Boost.Beast (C++)

**HTTP and WebSocket library built on Boost.Asio**

```cpp
#include <boost/beast/core.hpp>
#include <boost/beast/http.hpp>
#include <boost/beast/version.hpp>
#include <boost/asio/ip/tcp.hpp>
#include <iostream>

namespace beast = boost::beast;
namespace http = beast::http;
namespace net = boost::asio;
using tcp = boost::asio::ip::tcp;

// HTTP Client
void http_client() {
    net::io_context ioc;
    tcp::resolver resolver(ioc);
    beast::tcp_stream stream(ioc);
    
    auto const results = resolver.resolve("api.example.com", "80");
    stream.connect(results);
    
    http::request<http::string_body> req{http::verb::get, "/users", 11};
    req.set(http::field::host, "api.example.com");
    req.set(http::field::user_agent, BOOST_BEAST_VERSION_STRING);
    
    http::write(stream, req);
    
    beast::flat_buffer buffer;
    http::response<http::dynamic_body> res;
    http::read(stream, buffer, res);
    
    std::cout << res << std::endl;
    
    beast::error_code ec;
    stream.socket().shutdown(tcp::socket::shutdown_both, ec);
}
```

### Raw Socket HTTP Implementation

**Low-level HTTP using sockets**

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <netdb.h>
#include <string.h>
#include <stdio.h>
#include <unistd.h>

int http_get(const char *hostname, const char *path, int port) {
    int sockfd;
    struct sockaddr_in server_addr;
    struct hostent *server;
    char request[1024];
    char response[4096];
    
    // Create socket
    sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if (sockfd < 0) {
        perror("socket");
        return -1;
    }
    
    // Get host by name
    server = gethostbyname(hostname);
    if (server == NULL) {
        fprintf(stderr, "Error: no such host\n");
        return -1;
    }
    
    // Setup server address
    memset(&server_addr, 0, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    memcpy(&server_addr.sin_addr.s_addr, server->h_addr, server->h_length);
    server_addr.sin_port = htons(port);
    
    // Connect
    if (connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
        perror("connect");
        return -1;
    }
    
    // Build HTTP request
    snprintf(request, sizeof(request),
             "GET %s HTTP/1.1\r\n"
             "Host: %s\r\n"
             "Connection: close\r\n"
             "\r\n",
             path, hostname);
    
    // Send request
    if (send(sockfd, request, strlen(request), 0) < 0) {
        perror("send");
        return -1;
    }
    
    // Receive response
    ssize_t bytes_received = recv(sockfd, response, sizeof(response) - 1, 0);
    if (bytes_received < 0) {
        perror("recv");
        return -1;
    }
    
    response[bytes_received] = '\0';
    printf("%s\n", response);
    
    close(sockfd);
    return 0;
}

int main() {
    http_get("api.example.com", "/users", 80);
    return 0;
}
```

---

## 5. Common Interview Questions & Answers

### Q1: Explain the difference between HTTP and HTTPS.

**Answer:**
- **HTTP**: Unencrypted, port 80, no certificate required
- **HTTPS**: Encrypted with TLS/SSL, port 443, requires SSL certificate
- HTTPS provides confidentiality, integrity, and authentication

### Q2: What is the HTTP request/response cycle?

**Answer:**
1. Client sends HTTP request with method, URI, headers, body
2. Server processes request
3. Server sends HTTP response with status code, headers, body
4. Connection may close or persist (keep-alive)

### Q3: Explain HTTP methods and their idempotency.

**Answer:**
- **GET, HEAD, PUT, DELETE**: Idempotent (same result on multiple calls)
- **POST, PATCH**: Not idempotent
- **Safe methods**: GET, HEAD (no side effects)

### Q4: What are HTTP status codes? Give examples.

**Answer:**
- **2xx**: Success (200 OK, 201 Created, 204 No Content)
- **3xx**: Redirection (301 Moved, 302 Found, 304 Not Modified)
- **4xx**: Client error (400 Bad Request, 401 Unauthorized, 404 Not Found)
- **5xx**: Server error (500 Internal Error, 502 Bad Gateway, 503 Unavailable)

### Q5: What is HTTP keep-alive?

**Answer:**
HTTP keep-alive (persistent connections) allows multiple requests/responses over a single TCP connection, reducing:
- Connection establishment overhead
- Network latency
- Server resource usage

Enabled with `Connection: keep-alive` header.

### Q6: Explain HTTP caching.

**Answer:**
HTTP caching stores responses to reduce server load and latency:
- **Cache-Control**: Directives (max-age, no-cache, no-store)
- **ETag**: Entity tag for validation
- **Last-Modified**: Modification time
- **Expires**: Expiration time
- **304 Not Modified**: Server returns when resource unchanged

### Q7: What is CORS?

**Answer:**
**CORS (Cross-Origin Resource Sharing)** allows web pages to request resources from different domains:
- Browser enforces same-origin policy
- Server sends CORS headers (`Access-Control-Allow-Origin`)
- Preflight requests for complex requests

### Q8: Explain HTTP/2 features.

**Answer:**
- **Multiplexing**: Multiple streams over single connection
- **Header compression**: HPACK reduces overhead
- **Server push**: Server proactively sends resources
- **Binary protocol**: More efficient than text
- **Stream prioritization**: Important resources first

### Q9: What is the difference between PUT and PATCH?

**Answer:**
- **PUT**: Replaces entire resource (idempotent)
- **PATCH**: Partial update (may not be idempotent)

### Q10: Explain HTTP cookies.

**Answer:**
Cookies store state on client:
- **Set-Cookie**: Server sets cookie
- **Cookie**: Client sends cookie
- Attributes: Domain, Path, Expires, Secure, HttpOnly, SameSite

### Q11: What is HTTP pipelining?

**Answer:**
HTTP pipelining sends multiple requests without waiting for responses:
- Reduces latency
- Limited in HTTP/1.1 (head-of-line blocking)
- Improved in HTTP/2 with multiplexing

### Q12: How does HTTPS work?

**Answer:**
1. Client initiates TLS handshake
2. Server sends certificate
3. Client verifies certificate
4. Key exchange (RSA or ECDHE)
5. Symmetric encryption established
6. HTTP traffic encrypted

### Q13: What is HTTP content negotiation?

**Answer:**
Client and server negotiate content format:
- **Accept**: Preferred content types
- **Accept-Language**: Preferred languages
- **Accept-Encoding**: Compression support
- Server responds with appropriate format

### Q14: Explain HTTP chunked transfer encoding.

**Answer:**
Chunked encoding allows streaming responses:
- `Transfer-Encoding: chunked` header
- Body sent in chunks with size prefix
- Last chunk is size 0
- Useful for dynamic content

### Q15: What are HTTP security headers?

**Answer:**
- **Strict-Transport-Security (HSTS)**: Force HTTPS
- **Content-Security-Policy (CSP)**: Prevent XSS
- **X-Frame-Options**: Prevent clickjacking
- **X-Content-Type-Options**: Prevent MIME sniffing
- **X-XSS-Protection**: XSS protection

---

## Summary

**Key Takeaways:**
- HTTP is the foundation of web communication
- HTTPS adds encryption and security
- Understanding HTTP methods, status codes, and headers is essential
- Tools like curl and Wireshark are crucial for debugging
- Libraries like libcurl enable efficient HTTP programming

**For Zscaler Interviews:**
- Focus on HTTP/HTTPS protocol details
- Understand security implications
- Know common libraries and tools
- Be prepared to discuss performance optimization
- Understand proxy and firewall interactions

---

**Related Posts:**
- [Zscaler TCP/IP Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-tcp-ip-interview-preparation %})
- [Zscaler SSL/TLS Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-ssl-tls-interview-preparation %})
- [Zscaler QUIC Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-quic-interview-preparation %})

