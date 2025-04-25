# ip address

## description

The `ip address` command (also aliased `ip addr`) is part of the iproute2 suite for viewing and configuring IP addresses and related properties on network interfaces via the Linux kernel’s Netlink interface. When run without subcommands, it lists all interfaces and their configured addresses and associated details:
- link/ entry: link-layer information (MAC address, broadcast).
- inet: IPv4 address in CIDR notation, with optional fields like broadcast (`brd`), scope (`host`/`link`/`global`), and routing metric.
- inet6: IPv6 address in CIDR notation, including scope and flags (e.g., `dynamic`, `mngtmpaddr`).

Common operations:
- `ip address show [dev <interface>]`: display all or per-interface addresses.
- `ip address add <address>/<prefix> dev <interface>`: assign a new IP address.
- `ip address change|replace <address>/<prefix> dev <interface>`: modify an existing address.
- `ip address del <address>/<prefix> dev <interface>`: remove an IP address.

## example
```bash
$ ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 2c:cf:67:31:a2:08 brd ff:ff:ff:ff:ff:ff
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 2c:cf:67:31:a2:09 brd ff:ff:ff:ff:ff:ff
    inet 100.64.1.24/22 metric 600 brd 100.64.3.255 scope global dynamic wlan0
       valid_lft 446014sec preferred_lft 446014sec
    inet6 2400:4051:163:6300:2ecf:67ff:fe31:a209/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 2591996sec preferred_lft 604796sec
    inet6 fe80::2ecf:67ff:fe31:a209/64 scope link 
       valid_lft forever preferred_lft forever
4: br-16e6790e47fd: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether ee:04:bf:22:bf:0a brd ff:ff:ff:ff:ff:ff
    inet 192.168.49.1/24 brd 192.168.49.255 scope global br-16e6790e47fd
       valid_lft forever preferred_lft forever
5: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether d2:fc:0f:97:59:f9 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

Running `ip address show` lists all network interfaces and their addresses.  
The `inet` lines display each interface’s IPv4 address in CIDR form (`address/prefix-length`),  
along with optional fields like the broadcast address (`brd`) and the scope (`scope`).  
For example:  
```
inet 100.64.1.24/22 metric 600 brd 100.64.3.255 scope global wlan0
```  
means interface `wlan0` has IPv4 address `100.64.1.24/22`, broadcast `100.64.3.255`, metric 600, and global scope. 
