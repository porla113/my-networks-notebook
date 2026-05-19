# Connectivity Trobleshooting
- [Ping](#ping)
  - [Extended Ping](#extended-ping)
- [Traceroute](#traceroute)
- [Other Tools](#other-tools)

## Ping
- Ping tests two-way connectivity.
- ICMP: Internet Control Message Protocol
- ICMP Echo Request SRC IP: 10.0.0.1 -> DST IP: 10.1.0.1
- ICMP Echo Reply SRC IP: 10.1.0.1 -> DST IP: 10.0.0.1
- If ping is successful you will see `!`
- If ping is not successful you will see `.`
- It is common for the first ping to fail if the router is updating its ARP cache.
- If you see `.....`. The router does not have a corresponding route or the destination IP address is not responding.
- If you see `UUUUU`, U means unreachable. This happens because the router dicards the packet (i.e. being blocked by an ACL).

## Extended Ping
- When ping from a router, it will use the out going interface IP address as a source IP address. If we want to use another IP address we have to use extended ping.
- To use extended ping just type `ping`, it will then let you set options ex. protocol, target IP address, etc. 
- If you have been asked for extended command, type y (yes) so you can set the source IP address.
- Scenario:
![network-topology-shetch](/Networks/CCNA/Trobleshooting/images/ext_ping_img-001.jpg)
  - A user on PC1 complains that he can not access services on PC2
  - The real problem is R3 does not have a route to 10.0.1.0/24
  - If we ping PC2 from R1 (SRC: 10.0.0.1), it will succeed because R3 has a route to 10.0.0.1 
  - We can use an extended ping and use the issued subnet 10.0.1.1 as source address and 10.1.2.10 as target address.
  - The ping will fail because R3 does not have a path back to 10.0.1.1

## Traceroute
- It also use ICMP Echo Request/Reply
- And it use TTL (Time To Live) which is a field in IP header.
- How it works:
  - The first ping R1 sends with TTL 1.
    - ICMP Echo Request
    - SRC IP: 10.0.0.1
    - DEST IP: 10.1.0.1
    - TTL: 1
  - When the packet arrives at R2 and TTL is decremented by R2 to 0. R2 drops the packet and send an ICMP Time Exceeded message back to R1.
    - ICMP Time Exceeded
    - SRC IP: 10.0.0.2
    - DEST IP: 10.0.0.1
  - R1 learns that 10.0.0.2 (R2) is the first hop. and send another packet with TTL of 2.
    - ICMP Echo Request
    - SRC IP: 10.0.0.1
    - DEST IP: 10.1.0.1
    - TTL: 2
  - R3 decrement TTL to 0, drop the packet, and send an ICMP Time Exceeded to R1.
    - ICMP Time Exceeded
    - SRC IP: 10.1.0.1
    - DEST IP: 10.0.0.1
  - R1 reaches the destination, 10.1.0.1 (R3) and the traceroute is done.
- Sometimes the last hop will fail because firewall will normally drop traceroute traffic.
- If there is an issue, check the last hop in the result. Go to that router and `show ip route`. It is probably missing a route or might has some other issues.
- Press Ctrl-Shift-6 to abort.

## Other Tools
- Layer 1
  - `show ip interface brief`
  - `show interface`
- Layer 2
  - `show arp`
  - `show mac address-table`
- Layer 4
  - Telnet
- DNS
  - `nslookup`
  - Ping by FQDN