# Destination NA(P)Destination

## example

1. Create Network
```bash
$ sudo ip netns add lan
$ sudo ip netns add router
$ sudo ip netns add wan

$ sudo ip link add lan-veth0 type veth peer name gw-veth0
$ sudo ip link add wan-veth0 type veth peer name gw-veth1

$ sudo ip link set lan-veth0 netns lan
$ sudo ip link set gw-veth0 netns router
$ sudo ip link set gw-veth1 netns router
$ sudo ip link set wan-veth0 netns wan

$ sudo ip netns exec lan ip link set lan-veth0 up
$ sudo ip netns exec router ip link set gw-veth0 up
$ sudo ip netns exec router ip link set gw-veth1 up
$ sudo ip netns exec wan ip link set wan-veth0 up

$ sudo ip netns exec router ip address add 192.0.2.254/24 dev gw-veth0
$ sudo ip netns exec router ip address add 203.0.113.254/24 dev gw-veth1
$ sudo ip netns exec lan ip address add 192.0.2.1/24 dev lan-veth0
$ sudo ip netns exec lan ip route add default via 192.0.2.254
$ sudo ip netns exec wan ip address add 203.0.113.1/24 dev wan-veth0
$ sudo ip netns exec wan ip route add default via 203.0.113.254
```

2. Add NAT Rules
```bash
$ sudo ip netns exec router iptables -t nat \
-A PREROUTING \
-p tcp \
--dport 54321 \
-d 203.0.113.254 \
-j DNAT \
--to-destination 192.0.2.1
```

3. Check NAT Configuration
```bash
$ sudo ip netns exec router iptables -t nat -L
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination
DNAT       tcp  --  anywhere             203.0.113.254        tcp dpt:54321 to:192.0.2.1

Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination
MASQUERADE  all  --  192.0.2.0/24         anywhere
```

4. Create Server at `lan`
```bash
$ sudo ip netns exec lan nc -lnv 54321
Listening on 0.0.0.0 54321
```

5. Connect Server from `wan`

`wan`
```bash
$ sudo ip netns exec wan nc 203.0.113.254 54321
```

`lan`
```bash
$ sudo ip netns exec lan nc -lnv 54321
Listening on 0.0.0.0 54321
Connection received on 203.0.113.1 54758
```

6. Capture packet(`wan`)

`wan`(tcpdump)
```bash
$ sudo ip netns exec wan tcpdump -tnl -i wan-veth0 "tcp and port 54321"
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on wan-veth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
```

`wan`
```bash
$ sudo ip netns exec wan nc 203.0.113.254 54321
Hello, World!
```

`lan`
```bash
$ sudo ip netns exec lan nc -lnv 54321
...
Hello, World!
```

`wan`(tcpdump)
```bash
$ sudo ip netns exec wan tcpdump -tnl -i wan-veth0 "tcp and port 54321"
...
IP 203.0.113.1.54758 > 203.0.113.254.54321: Flags [P.], seq 820308212:820308226, ack 1813569330, win 502, options [nop,nop,TS val 4003912978 ecr 266921548], length 14
IP 203.0.113.254.54321 > 203.0.113.1.54758: Flags [.], ack 14, win 509, options [nop,nop,TS val 267183463 ecr 4003912978], length 0
```

7. Capture packet(`lan`)

send `Hello, World!` from `wan` to `lan`, as well.

`lan`(tcpdump)
```bash
$ sudo ip netns exec lan tcpdump -tnl -i lan-veth0 "tcp and port 54321"
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on lan-veth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
IP 203.0.113.1.54758 > 192.0.2.1.54321: Flags [P.], seq 820308226:820308240, ack 1813569330, win 502, options [nop,nop,TS val 4004201895 ecr 267183463], length 14
IP 192.0.2.1.54321 > 203.0.113.1.54758: Flags [.], ack 14, win 509, options [nop,nop,TS val 267472380 ecr 4004201895], length 0
```
