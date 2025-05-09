# HTTP

## descriptions

HTTP (Hypertext Transfer Protocol) is the application-layer protocol underlying the World Wide Web. It follows a client–server model: a client (such as a web browser) sends a request—consisting of a request-line (method, URI, version), headers, and optional body—to a server, which then returns a response with a status-line (version, status code, reason phrase), headers, and optional body.

Key features:
- Common methods:
  - GET (retrieve resource)
  - POST (submit data)
  - PUT (update resource)
  - DELETE (remove resource)
  - HEAD (headers only)
  - OPTIONS (query available methods)
- Status codes grouped by class:
  - 1xx (informational), 2xx (success), 3xx (redirection), 4xx (client error), 5xx (server error)
- Statelessness: each request is independent; state (sessions, authentication) is managed via cookies or tokens
- Evolution:
  - HTTP/1.1 added persistent connections and chunked transfer encoding
  - HTTP/2 introduced multiplexing, header compression, and binary framing for performance

## example

1. Create HTML file

```index.html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello, World</title>
</head>

<body>
    <h1> Hello, World</h1>
</body>

</html>
```

2. Launch HTTP Server

`tabA`
```bash
$ cd <path>/<to>/<dir>/<of>/<index.html>
$ sudo python3 -m http.server -b 127.0.0.1 80
Serving HTTP on 127.0.0.1 port 80 (http://127.0.0.1:80/) ...
```

3. Echo HTTP Server

`tabB`
```bash
$ echo -en "GET / HTTP/1.0\r\n\r\n" | nc 127.0.0.1 80
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.12.3
Date: Wed, 07 May 2025 12:24:09 GMT
Content-type: text/html
Content-Length: 234
Last-Modified: Wed, 07 May 2025 12:18:40 GMT

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello, World</title>
</head>

<body>
    <h1> Hello, World</h1>
</body>
```

`tabA`
```bash
$ sudo python3 -m http.server -b 127.0.0.1 80
Serving HTTP on 127.0.0.1 port 80 (http://127.0.0.1:80/) ...
127.0.0.1 - - [07/May/2025 21:24:09] "GET / HTTP/1.0" 200 -
```
