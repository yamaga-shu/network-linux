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

```
