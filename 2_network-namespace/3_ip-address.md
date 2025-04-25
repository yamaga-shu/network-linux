# IP Address

## description
IPv4 addresses are 32-bit values typically shown in dotted-decimal notation as four octets (8-bit binary groups), e.g. `192.168.1.10`. Each octet is converted from binary to decimal (0–255) by summing the powers of two for bits set to 1.

The address space is divided into network and host portions using a subnet mask or CIDR prefix:
- Subnet mask (dotted-decimal, e.g. `255.255.255.0`) or CIDR `/n` (e.g. `/24`) indicates how many leading bits belong to the network.
- Network address = IP address bitwise AND mask.
- Host identifier = remaining bits after the network portion.

Example calculation for `192.168.1.10/24`:
```
IP 192.168.1.10 → 11000000.10101000.00000001.00001010
Mask 255.255.255.0 → 11111111.11111111.11111111.00000000
& 192.168.1.0 → 11000000.10101000.00000001.00000000
```

Network segment: `192.168.1.0`  Host ID: `10`
