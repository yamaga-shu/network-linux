# ping

## description

Ping is a command-line network diagnostic tool that sends ICMP Echo Request packets to a target host and waits for Echo Reply responses. It reports each packet’s round-trip time, TTL, and sequence number, and summarizes packet loss and overall latency statistics.

## example
```bash
$ ping -c 3 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=119 time=18.9 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=119 time=4.69 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=119 time=7.61 ms

--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 4.692/10.396/18.886/6.120 ms
```

### IP Address
`8.8.8.8` is an IP address in the command above. It is one of the identifiers used to transfer information via IP (Internet Protocol). IP is a fundamental protocol of the Internet.

An IP address is unique on the Internet, meaning there are no duplicate IP addresses. Therefore, `8.8.8.8` represents Google's public DNS server.

> [!IMPORTANT]
> Only global IP addresses should be unique on the Internet.
>
> Other IP addresses, such as private IPs used in closed networks (home, organization, etc.), may be duplicated.
