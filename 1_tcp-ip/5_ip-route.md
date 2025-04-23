# ip route

## description
The `ip route` command (alias `ip r`) is part of the iproute2 suite for viewing and manipulating the kernel routing table via the Linux kernel’s Netlink interface. When run without subcommands (`ip route show`), it lists all active routes along with key fields:
- destination prefix (e.g., `default`, `100.64.0.0/22`)
- next-hop gateway (`via <gateway>`)
- output interface (`dev <iface>`)
- protocol (`proto <value>`), metric, scope, and source address (`src`)

Common operations:
- `ip route show [dev <iface>]`: display all or per-interface routes  
- `ip route add <dest>/<prefix> via <gateway> dev <iface> [metric <n>]`: add a new route  
- `ip route change <dest>/<prefix> ...`: modify an existing route  
- `ip route del <dest>/<prefix>`: remove a route

## example
```
$ ip route show
default via 100.64.1.1 dev wlan0 proto dhcp src 100.64.1.24 metric 600 
8.8.8.8 via 100.64.1.1 dev wlan0 proto dhcp src 100.64.1.24 metric 600 
100.64.0.0/22 dev wlan0 proto kernel scope link src 100.64.1.24 metric 600 
100.64.1.1 dev wlan0 proto dhcp scope link src 100.64.1.24 metric 600 
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown 
192.168.49.0/24 dev br-16e6790e47fd proto kernel scope link src 192.168.49.1 linkdown 
208.67.222.222 via 100.64.1.1 dev wlan0 proto dhcp src 100.64.1.24 metric 600
```