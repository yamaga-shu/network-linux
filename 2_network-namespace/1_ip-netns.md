# ip netns

## description

The `ip netns` command is part of the iproute2 suite for managing Linux network namespaces—isolated network stacks that include interfaces, routing tables, and firewall rules. Each namespace provides a separate networking environment for processes, enabling containerization, testing, and traffic isolation on the same host.

Common operations:
- `ip netns add <name>`: create a new network namespace.
- `ip netns list`: list all existing network namespaces.
- `ip netns exec <name> <cmd>`: execute a command inside the specified namespace.
- `ip netns delete <name>`: remove a network namespace (namespace must be empty).

## example
1. Create Network Namespace
```
$ sudo ip netns add helloworld
$ ip netns list
helloworld
```

2. Execute command in Created one.
```
$ sudo ip netns exec helloworld ip address show
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
$ sudo ip netns exec helloworld ip route show
$
```

3. Delete Network Namespace
```
$ sudo ip netns delete helloworld 
```