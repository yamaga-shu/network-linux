# DNS(Domain Name System)

## description

DNS (Domain Name System) is a hierarchical, distributed naming service that maps human-readable domain names (e.g. example.com) to IP addresses (IPv4 or IPv6). It relies on a network of name servers (root, TLD, authoritative) and resolvers (stub or recursive) to answer queries.

Key resource record types:
- A: maps a domain name to an IPv4 address  
- AAAA: maps a domain name to an IPv6 address  
- CNAME: creates an alias from one name to another canonical name  
- NS: specifies authoritative name servers for a zone  
- MX: identifies mail servers for a domain  
- TXT: holds descriptive text or verification data (e.g., SPF, DKIM)  
- SOA: start of authority, provides zone metadata (primary server, contact, serial, TTL)

Resolution process:
1. Resolver issues recursive or iterative query  
2. Root servers point to the appropriate TLD servers  
3. TLD servers point to the domain’s authoritative servers  
4. Authoritative servers return the requested record  
5. Resolver caches the response for the record’s TTL duration

## example

1. Observing
```
$ sudo tcpdump -tnl -i any "udp and port 53"
tcpdump: data link type LINUX_SLL2
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on any, link-type LINUX_SLL2 (Linux cooked v2), snapshot length 262144 bytes
```

2. Digging
```
$ dig +short @8.8.8.8 example.org A
23.215.0.132
96.7.128.192
96.7.128.186
23.215.0.133
```

```
$ sudo tcpdump -tnl -i any "udp and port 53"
...

wlan0 Out IP 100.64.1.24.46223 > 8.8.8.8.53: 58837+ [1au] A? example.org. (52)
wlan0 In  IP 8.8.8.8.53 > 100.64.1.24.46223: 58837$ 4/0/1 A 23.215.0.132, A 96.7.128.192, A 96.7.128.186, A 23.215.0.133 (104)
```
