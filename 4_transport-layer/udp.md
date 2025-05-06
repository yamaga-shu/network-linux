# UDP

## description

User Datagram Protocol (UDP) is a connectionless, unreliable protocol operating at the transport layer (Layer 4 of the OSI model). Unlike TCP, UDP does not establish a connection before sending data, nor does it guarantee delivery, ordering, or duplicate protection. This minimal overhead makes UDP highly efficient and suitable for applications that require low latency or can tolerate occasional packet loss—such as DNS queries, video streaming, VoIP, and online gaming. Each UDP datagram contains only a checksum, source and destination ports, length field, and payload. While UDP’s lack of reliability features reduces overhead, it places responsibility for error detection and recovery on the application layer when needed.

## nc(Netcat)

Netcat (`nc`) is a lightweight command-line utility for reading from and writing to network connections using TCP or UDP. It can act as either a client (connecting to a remote host) or a server (listening for inbound connections). In the examples above we use:

- `-u`: use UDP instead of the default TCP  
- `-l`: listen for an incoming connection (server mode)  
- `-n`: skip DNS resolution; use numeric IP addresses only  
- `-v`: verbose output, showing connection and packet details  

Here, the server runs `nc -ulnv <host> <port>` to listen for UDP datagrams, and the client sends messages with `nc -u <host> <port>`.

## example

1. Create Server

`tabA`
```
$ nc -ulnv 127.0.0.1 54321
Bound on 127.0.0.1 54321
```

2. Create Client

`tabB`
```
$ nc -u 127.0.0.1 54321
```

3. Prepare Transport Capturing

`tabC`
```
$ sudo tcpdump -i lo -tnlA "udp and port 54321"
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lo, link-type EN10MB (Ethernet), snapshot length 262144 bytes
```

4. Send message to Server from Client

`tabB`
```
$ nc -u 127.0.0.1 54321
Hello, World!
```

`tabA`
```
$ nc -ulnv 127.0.0.1 54321
Bound on 127.0.0.1 54321
Connection received on 127.0.0.1 50060
Hello, World!
```

`tabC`
```
$ sudo tcpdump -i lo -tnlA "udp and port 54321"

...

IP 127.0.0.1.50060 > 127.0.0.1.54321: UDP, length 14
E..*..@.@..............1...)Hello, World!
```

5. Reply from Server

`tabA`
```
$ nc -ulnv 127.0.0.1 54321

...

Hello, World!
Reply, World!
```

`tabB`
```
$ nc -u 127.0.0.1 54321
Hello, World!
Reply, World!
```

`tabC`
```
$ sudo tcpdump -i lo -tnlA "udp and port 54321"

...

IP 127.0.0.1.54321 > 127.0.0.1.50060: UDP, length 14
E..*J.@.@............1.....)Reply, World!
```