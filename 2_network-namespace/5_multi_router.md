# multi router

## example

![Network Configration Diagram(Physical)](./assets/multi-router-network-physical.drawio.png)

![Network Configration Diagram(Logical)](./assets/multi-router-network-logical.drawio.png)

1. Create Network Namespaces
```
$ sudo ip netns add ns1
$ sudo ip netns add router1
$ sudo ip netns add router2
$ sudo ip netns add ns2
$ sudo ip netns show
ns1
router1
router2
ns2
```

2. Create veth interfaces
```
$ sudo ip link add ns1-veth0 type veth peer name gw1-veth0
$ sudo ip link add gw1-veth1 type veth peer name gw2-veth0
$ sudo ip link add gw2-veth1 type veth peer name ns2-veth0
```

3. Attach veth interfaces on Network Namespaces
```
$ sudo ip link set ns1-veth0 netns ns1
$ sudo ip link set gw1-veth0 netns router1
$ sudo ip link set gw1-veth1 netns router1
$ sudo ip link set gw2-veth0 netns router2
$ sudo ip link set gw2-veth1 netns router2
$ sudo ip link set ns2-veth0 netns ns2
```

4. Set the state of veth interfaces UP
```
$ sudo ip netns exec ns1 ip link set ns1-veth0 up
$ sudo ip netns exec router1 ip link set gw1-veth0 up
$ sudo ip netns exec router1 ip link set gw1-veth1 up
$ sudo ip netns exec router2 ip link set gw2-veth0 up
$ sudo ip netns exec router2 ip link set gw2-veth1 up
$ sudo ip netns exec ns2 ip link set ns2-veth0 up
```

5. Allocate IP Addresses
```
$ sudo ip netns exec ns1 ip address add 192.0.2.1/24 dev ns1-veth0

$ sudo ip netns exec router1 ip address add 192.0.2.254/24 dev gw1-veth0
$ sudo ip netns exec router1 ip address add 203.0.113.1/24 dev gw1-veth1

$ sudo ip netns exec router2 ip address add 203.0.113.2/24 dev gw2-veth0
$ sudo ip netns exec router2 ip address add 198.51.100.254/24 dev gw2-veth1

$ sudo ip netns exec ns2 ip address add 198.51.100.1/24 dev ns2-veth0
```

6. Check the connection of each node

`ns1 <-> router1`
```
$ sudo ip netns exec ns1 ping -c 3 192.0.2.254 -I 192.0.2.1
(result here)
```

`router1 <-> router2`
```
$ sudo ip netns exec router1 ping -c 3 203.0.113.2 -I 203.0.113.1
(result here)
```

`router2 <-> ns2`
```
$ sudo ip netns exec router2 ping -c 3 198.51.100.1 -I 198.51.100.254
(result here)
```

7. Check the connection of each namespace through the router (failed)
```
$ sudo ip netns exec ns1 ping -c 3 198.51.100.1 -I 192.0.2.1
(result here)
```

8. Add default route to each namespace
```
$ sudo ip netns exec ns1 ip route add default via 192.0.2.254
$ sudo ip netns exec ns2 ip route add default via 198.51.100.254
```

9. Add diffrerent segment route to each router
```
$ sudo ip netns exec router1 ip route add 198.51.100.0/24 via 203.0.113.2
$ sudo ip netns exec router2 ip route add 192.0.2.0/24 via 203.0.113.1
```

10. Check the connection of each namespace through the router
```
$ sudo ip netns exec ns1 ping -c 3 198.51.100.1 -I 192.0.2.1
(result here)
```