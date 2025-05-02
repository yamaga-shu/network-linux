# MAC Address

## description

(description here)

## example
1. Create Network Namespace
```
$ sudo ip netns add ns1
$ sudo ip netns add ns2
```

2. Create veth
```
$ sudo ip link add ns1-veth0 type veth peer name ns2-veth0
$ ip link show | grep veth
20: ns2-veth0@ns1-veth0: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
21: ns1-veth0@ns2-veth0: <BROADCAST,MULTICAST,M-DOWN> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
```

3. Attach veth on Network Namespace
```
$ sudo ip link set ns1-veth0 netns ns1
$ sudo ip link set ns2-veth0 netns ns2
```

![Physical Diagram](./assets/mac-address-physical.drawio.png)

4. Set IP address on each veth
```
$ sudo ip netns exec ns1 ip address add 192.0.2.1/24 dev ns1-veth0
$ sudo ip netns exec ns2 ip address add 192.0.2.2/24 dev ns2-veth0
```

5. Set veth's state UP
```
$ sudo ip netns exec ns1 ip link set ns1-veth0 up
$ sudo ip netns exec ns2 ip link set ns2-veth0 up
```
