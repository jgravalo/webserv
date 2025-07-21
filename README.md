# 🌐 Webserv

**Webserv** is a lightweight HTTP/1.1-compliant server written in C++98. It is designed to handle multiple client connections concurrently using a non-blocking I/O model and minimal system resources. The goal of this project is to deeply understand the mechanics of HTTP communication and server architecture, by building an actual HTTP server from scratch.

This program parses a configuration file, similar to that in Nginx, sets up and binds the listening sockets, accepts incoming connections, handles each single I/O interaction with the client flow through a poll implementation and can receive and serve GET, DELETE and POST HTTP requests.

---

## 📦 Project Overview

- **Language**: C++98
- **Standard**: -Wall -Wextra -Werror -std=c++98
- **I/O Model**: `poll()` (or `select()`, `kqueue()`, or `epoll()`)
- **OS Compatibility**: Linux and macOS (with specific `fcntl()` flags allowed)
- **Build System**: Makefile

---
🔧 Features
- ✅ Non-blocking I/O using a single poll() call
- ✅ Multiple server blocks listening on different ports and IPs
- ✅ Custom routing and location blocks
- ✅ Static file serving (HTML, images, etc.)
- ✅ File upload via POST
- ✅ CGI support (e.g., PHP, Python scripts)
- ✅ Configurable error pages
- ✅ Accurate HTTP status codes and headers
- ✅ Directory listing (optional per location)
- ✅ Method restrictions (GET, POST, DELETE)
- ✅ HTTP redirects
- ✅ Default file serving for directories
- ✅ Stress-test ready & resilient

---
## 🏁 How to Run

```bash

./webserv

# it calls the default server at ./conf/default.conf

./webserv [configuration file]

# another configuration file specified
```
Then just access the server via curl or browser at the port specified in the "Listen" directive, and you will be served, considering that the "Root" directive points to the server root.

For example, if:

```
server {
     listen 8888;
     root .;
}
server {
    listen 127.0.0.1:8080;
    server_name localhost;

    error_page 404 /404.html;

    client_max_body_size 1M;

    location / {
        root /var/www/html;
        index index.html;
        autoindex on;
        methods GET POST;
    }

    location /upload {
        root /var/www/uploads;
        methods POST;
        upload_store /tmp/uploads;
    }

    location /cgi-bin {
        cgi_pass /usr/bin/php-cgi;
        extensions .php;
    }
}


```

Doing a:
```
curl 127.0.0.1:8888
```
Will make a GET request to the root of the server, and will receive an index page or an error if no index page was found.

## Resources

This guide was really fun and easy to follow through. I highly recommend it as a first read:

[Beej's Guide To Network Programming](https://beej.us/guide/bgnet/html/split/)

Reading the HTTP RFC documents was essential for tackling the HTTP requirements of the project:

[RFC 9112 - Hypertext Transfer Protocol](https://datatracker.ietf.org/doc/html/rfc9112)

[RFC 9110 - HTTP Semantics](https://datatracker.ietf.org/doc/html/rfc9110)
