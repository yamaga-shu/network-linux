# HTTP

## descriptions


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
```
$ cd <path>/<to>/<dir>/<of>/<index.html>
$ sudo python3 -m http.server -b 127.0.0.1 80
Serving HTTP on 127.0.0.1 port 80 (http://127.0.0.1:80/) ...
```

3. Echo HTTP Server

`tabB`
```
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
```
$ sudo python3 -m http.server -b 127.0.0.1 80
Serving HTTP on 127.0.0.1 port 80 (http://127.0.0.1:80/) ...
127.0.0.1 - - [07/May/2025 21:24:09] "GET / HTTP/1.0" 200 -
```