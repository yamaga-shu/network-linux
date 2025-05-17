# socket / http

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

3. Create HTTP Client(client.py)

```client.py
#! /usr/bin/env python3

import socket

def send_msg(sock, msg):
    """ソケットに指定したバイト列を書き込む関数"""
    # これまでに送信できたバイト数
    total_sent_len = 0
    # 送信したいバイト数
    total_msg_len = len(msg)
    # まだ送信したいデータが残っているか判定する
    while total_sent_len < total_msg_len:
        # ソケットにバイト列を書き込んで、書き込めたバイト数を得る
        sent_len = sock.send(msg[total_sent_len:])
        # 全く書き込めなかったらソケットの接続が終了している
        if sent_len == 0:
            raise RuntimeError('socket connection broken')
        # 書き込めた分を加算する
        total_sent_len += sent_len

def recv_msg(sock, chunk_len=1024):
    """ソケットから接続が終わるまでバイト列を読み込むジェネレータ関数"""
    while True:
        # ソケットから指定したバイト数を読み込む
        received_chunk = sock.recv(chunk_len)
        # 全く読めなかった時は接続が終了している
        if len(received_chunk) == 0:
            break
        # 受信したバイト列を返す
        yield received_chunk


def main():
    """スクリプトとして実行されたときに呼び出されるメイン関数"""
    # IPv4 / TCP で通信するソケットを用意する
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # ループバックアドレスの TCP/80 ポートに接続する
    client_socket.connect(('127.0.0.1', 80))
    # HTTP サーバからドキュメントを取得するための GET リクエスト
    request_text = 'GET / HTTP/1.0\r\n\r\n'
    # 文字列をバイト列にエンコードする
    request_bytes = request_text.encode('ASCII')
    # ソケットにリクエストのバイト列を書き込む
    send_msg(client_socket, request_bytes)
    # ソケットからレスポンスのバイト列を読み込む
    received_bytes = b''.join(recv_msg(client_socket))
    # 読み込んだバイト列を文字列にデコードする
    received_text = received_bytes.decode('ASCII')
    # 得られた文字列を表現する
    print(received_text)
    # 使い終わったソケットを閉じる
    client_socket.close()

if __name__ == '__main__':
    """スクリプトのエントリーポイントとしてメイン関数を実行する"""
    main()
```

4. Echo HTTP Server by python

```bash
$ python3 client.py
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.12.3
Date: Wed, 14 May 2025 23:14:26 GMT
Content-type: text/html
Content-Length: 234
Last-Modified: Wed, 14 May 2025 23:05:53 GMT

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

```bash
$ sudo python3 -m http.server -b 127.0.0.1 80
Serving HTTP on 127.0.0.1 port 80 (http://127.0.0.1:80/) ...
127.0.0.1 - - [15/May/2025 08:14:26] "GET / HTTP/1.0" 200 -
```
