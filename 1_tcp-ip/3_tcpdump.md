# tcpdump

## description

Tcpdump is a command-line packet analyzer that captures and displays network packets in real time by leveraging the libpcap library. You can apply capture filters (e.g. `icmp`, IP addresses, ports) to focus on specific traffic. Each output line shows:
- interface and direction (`In`/`Out`)
- protocol (IP, ICMP, TCP/UDP, etc.)
- source and destination addresses
- packet-specific details (type, id, seq, length, ports)

Common options:
- `-i <interface>`: listen on a given interface (`any` for all)
- `-n`: numeric output only (no DNS/port name resolution)
- `-t`: omit timestamps
- `-c <count>`: stop after capturing a set number of packets
- `-v` / `-vv`: increase verbosity for more header details

## example
1. `sudo tcpdump -tn -i any icmp`(tab A)
2. open another shell tab(tab B)
3. `ping -c 3 8.8.8.8`

`tab A`
```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=119 time=7.04 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=119 time=5.00 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=119 time=7.65 ms

--- 8.8.8.8 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 4.997/6.561/7.646/1.133 ms
```

`tab B`
```
wlan0 Out IP 100.64.1.24 > 8.8.8.8: ICMP echo request, id 30456, seq 1, length 64
wlan0 In  IP 8.8.8.8 > 100.64.1.24: ICMP echo reply, id 30456, seq 1, length 64
wlan0 Out IP 100.64.1.24 > 8.8.8.8: ICMP echo request, id 30456, seq 2, length 64
wlan0 In  IP 8.8.8.8 > 100.64.1.24: ICMP echo reply, id 30456, seq 2, length 64
wlan0 Out IP 100.64.1.24 > 8.8.8.8: ICMP echo request, id 30456, seq 3, length 64
wlan0 In  IP 8.8.8.8 > 100.64.1.24: ICMP echo reply, id 30456, seq 3, length 64
```

4. `ping -c 3 127.0.0.1`

`tab A`
```
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.063 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.040 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.036 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2046ms
rtt min/avg/max/mdev = 0.036/0.046/0.063/0.011 ms
```

`tab B`
```
lo    In  IP 127.0.0.1 > 127.0.0.1: ICMP echo request, id 30462, seq 1, length 64
lo    In  IP 127.0.0.1 > 127.0.0.1: ICMP echo reply, id 30462, seq 1, length 64
lo    In  IP 127.0.0.1 > 127.0.0.1: ICMP echo request, id 30462, seq 2, length 64
lo    In  IP 127.0.0.1 > 127.0.0.1: ICMP echo reply, id 30462, seq 2, length 64
lo    In  IP 127.0.0.1 > 127.0.0.1: ICMP echo request, id 30462, seq 3, length 64
lo    In  IP 127.0.0.1 > 127.0.0.1: ICMP echo reply, id 30462, seq 3, length 64
```

- Running `sudo tcpdump -tn -i any icmp` captures all ICMP packets on every interface, printing numeric addresses only and disabling DNS lookups.
- Each output line follows the pattern:  
  `<interface> <direction> IP <source> > <destination>: ICMP <type>, id <id>, seq <seq>, length <length>`
- In the first example (ping to 8.8.8.8), `Out` entries are the echo requests and `In` entries are the replies on the `wlan0` interface.
- In the second example (ping to 127.0.0.1), both request and reply stay on the loopback interface (`lo`), so all packets are labeled `In` on `lo`.

> [!NOTE]  
> 127.0.0.1 is the loopback (localhost) address, so ping round‑trip times are very short.

### `tcpdump`
```
tcpdump version 4.99.4
libpcap version 1.10.4 (with TPACKET_V3)
OpenSSL 3.0.13 30 Jan 2024
Usage: tcpdump [-AbdDefhHIJKlLnNOpqStuUvxX#] [ -B size ] [ -c count ] [--count]
		[ -C file_size ] [ -E algo:secret ] [ -F file ] [ -G seconds ]
		[ -i interface ] [ --immediate-mode ] [ -j tstamptype ]
		[ -M secret ] [ --number ] [ --print ] [ -Q in|out|inout ]
		[ -r file ] [ -s snaplen ] [ -T type ] [ --version ]
		[ -V file ] [ -w file ] [ -W filecount ] [ -y datalinktype ]
		[ --time-stamp-precision precision ] [ --micro ] [ --nano ]
		[ -z postrotate-command ] [ -Z user ] [ expression ]
```