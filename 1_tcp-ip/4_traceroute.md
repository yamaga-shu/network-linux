# traceroutes

## description
Traceroute is a network diagnostic tool that shows the path packets take to reach a destination by sending probe packets with incrementally increasing TTL (Time To Live) values. Each router along the path decrements the TTL by one; when the TTL reaches zero, the router discards the packet and replies with an ICMP “Time Exceeded” message, revealing its IP address. By collecting these messages hop by hop, traceroute maps out the entire route.

- Router: a network device that forwards packets between different networks by examining the destination IP address and consulting its routing table.
- How it works (TTL increment): traceroute starts by sending probes with TTL=1; the first router decrements TTL to 0, drops the packet, and sends back ICMP Time Exceeded. Traceroute records that router’s IP, then sends probes with TTL=2 to reach the second hop, and so on, until it reaches the target or the maximum hop count.

## example
```bash
$ traceroute -n 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  100.64.1.1  2.203 ms  2.278 ms  2.259 ms
 2  180.8.126.94  5.542 ms 180.8.126.14  4.869 ms 180.8.126.70  4.892 ms
 3  180.8.126.101  9.010 ms 180.8.126.61  9.607 ms 180.8.126.9  5.971 ms
 4  211.0.193.22  8.028 ms  8.148 ms 211.129.53.2  5.912 ms
 5  108.170.248.185  8.081 ms 108.170.231.103  7.076 ms *
 6  8.8.8.8  7.949 ms 216.239.41.71  6.375 ms 209.85.253.109  8.825 ms
```
