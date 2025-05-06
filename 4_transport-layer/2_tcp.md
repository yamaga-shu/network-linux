# TCP

## description

Transmission Control Protocol (TCP) is a connection-oriented transport protocol that establishes a reliable, in-order byte stream between two endpoints. It begins with a three-way handshake to synchronize sequence numbers, then uses acknowledgments, retransmissions, and checksums to guarantee delivery. Flow control via a sliding window prevents receiver overload, while congestion control algorithms—slow start, congestion avoidance, fast retransmit, and fast recovery—adapt the sender’s rate to network conditions. When the session ends, a four-step FIN/ACK exchange cleanly closes the connection. Optional extensions such as window scaling, selective acknowledgments (SACK), and timestamps further optimize performance over high-latency or high-bandwidth links.

### control bits

TCP’s control flags are carried in the 8-bit Flags field of the TCP header, immediately following the 4-bit Data Offset and 3-bit Reserved fields. A simplified view of that portion of the header looks like this:

| Data Offset | Reserved | NS | CWR | ECE | URG | ACK | PSH | RST | SYN | FIN |
| ----------- | -------- | -- | --- | --- | --- | --- | --- | --- | --- | --- |
| 4 bits | 3 bits | 1 bit | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |

- `SYN` (Synchronize): signals the start of a new connection and carries the sender’s initial sequence number.  
- `ACK` (Acknowledgment): indicates that the acknowledgment field is valid and confirms receipt of data or control segments.  
- `FIN` (Finish): initiates a graceful shutdown by signaling that the sender has no more data to transmit.  
- `RST` (Reset): forces an immediate termination of the connection, typically used to signal errors or unexpected conditions.  
- `PSH` (Push): instructs the receiver to deliver buffered data to the application immediately rather than waiting to fill the buffer.  
- `URG` (Urgent): marks the segment as containing urgent data and makes the urgent pointer field significant for prioritized delivery.  

### three-way handshake

When a TCP endpoint initiates a connection, it performs a three-step exchange using the SYN and ACK control bits:

1. Client → Server: SYN=1, ACK=0, seq=C_ISN  
   The client sends a segment with the SYN (synchronize) bit set and selects an initial sequence number (C_ISN).

2. Server → Client: SYN=1, ACK=1, seq=S_ISN, ack=C_ISN + 1  
   The server replies with both SYN and ACK bits set. The SYN announces the server’s ISN (S_ISN), and the ACK acknowledges the client’s SYN by echoing C_ISN + 1.

3. Client → Server: SYN=0, ACK=1, seq=C_ISN + 1, ack=S_ISN + 1  
   The client sends a final ACK segment, confirming receipt of the server’s SYN. With both sides having seen each other’s sequence numbers, the connection enters the ESTABLISHED state.

After this handshake, both endpoints are synchronized on sequence numbers and ready for reliable, in-order data transfer.

## example

1. Create Server

`tabA`
```
$ nc -lnv 127.0.01 54321
Listening on 127.0.0.1 54321
```

2. Prepare Segment Capturing

`tabB`
```
$ sudo tcpdump -i lo -tnlA "tcp and port 54321"
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lo, link-type EN10MB (Ethernet), snapshot length 262144 bytes
```

3. Create Client

`tabC`
```
$ nc 127.0.0.1 54321
```

`tabA`
```
$ nc -lnv 127.0.01 54321
Listening on 127.0.0.1 54321
Connection received on 127.0.0.1 44900
```

`tabB`
```
$ sudo tcpdump -i lo -tnlA "tcp and port 54321"

...

IP 127.0.0.1.44900 > 127.0.0.1.54321: Flags [S], seq 4161610494, win 65495, options [mss 65495,sackOK,TS val 293076160 ecr 0,nop,wscale 7], length 0
E..<.J@.@.%p.........d.1.."..........0.........
.w..........
IP 127.0.0.1.54321 > 127.0.0.1.44900: Flags [S.], seq 3403849393, ack 4161610495, win 65483, options [mss 65495,sackOK,TS val 293076160 ecr 293076160,nop,wscale 7], length 0
E..<..@.@.<..........1.d......"......0.........
.w...w......
IP 127.0.0.1.44900 > 127.0.0.1.54321: Flags [.], ack 1, win 512, options [nop,nop,TS val 293076160 ecr 293076160], length 0
E..4.K@.@.%w.........d.1.."..........(.....
.w...w..
```
