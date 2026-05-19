# Dynamic Routing Protocols
- [Overview](#overview)
- [Protocols](#protocols)
- [Metric](#metric)
- [ECMP](#ecmp-equal-cost-multi-path)
- [AD](#ad-administrative-distance)
- [Floating Static Routes](#floating-static-routes)
- [Loopback Interfaces](#loopback-interfaces)
- [Adjacencies](#adjacencies)
- [Route Precedence](#route-precedence)

## Overview
- Dynamic routing protocols vs static routes
  - Routing protocols are more scalable.
  - Using static routes is for small environments.
  - However, using both is common in real world environments.
    - The routing protocol will be used to carry the bulk of the network information.
    - The static routes can be used for backup purposes or a static route to the internet
- How it work.
  - When routing protocol is used, routers automatically advertise their best paths to known networks to each other.
  - Routers use this information to determine their own best path to the known destinations.
  - When the state of the network changes (links go down or subnets being added), the routers update each other.
  - Routers will automatically calculate a new best path and update the routing table if the network changes.
## Protocols
- **Dynamic Routing Protocols**
  - **IGPs** (Interior Gateway Protocols)
    - **Distance Vector Routing Protocols**
      - **RIP** (Routing Information Protocol)
      - **EIGRP** (Enhanced Interior Gateway Routing Protocol)
    - **Link State Routing Protocols**
      - **OSPF** (Open Shortest Path First)
      - **IS-IS** (Intermediate System - Intermediate System)
  - **EGPs** (Exterior Gateway Protocols)
    - **BGP** (Border Gateway Protocol)
---
- **IGPs** are used for routing within an organization.
- **EGPs** are used for routing between organizations over the Internet.
- The only **EGP** in use today is **BGP**
- **Distance Vector protocols**
  - They do not advertise the entire network topology.
  - A router only knows its directly connected neighbours and the lists of networks those neighbors have advertised. It does not have detailed topology information beyond its directly connected neighbors.
  - They are often called **'Routing by rumour'**.
- **Link State routing protocols**
  - Each router describes itself and its interfaces to its directly connected neighbors.
  - The information is passed unchanged from one router to another.
  - Every router learns the full picture of the network including every router, its interfaces and what they connect to.
- You can have a multiple routing protocols running on the same router, but it is **not recommended**. 
  - Check with `show ip protocols` / `sh run | section rip` / `sh run | begin rip`
  - `sh ip rip database` / `sh ip route`
## Metric
- A router may receive multiple possible paths to a destination network.
- Only the best path will be put into the routing table and used.
- The different IGPs use different methods to calculate the best path.
- The lowest metric is preferred ("cost", cheaper is better).
- If the best path to a destination is lost, it will be removed from the routing table and replaced with the next best route.
- **RIP**
  - Uses "Hop Count" as the metric.
  - The maximum hop count by default is 15. Paths with more than 15 hops are marked as unreachable.
  - It does not take bandwidth into account. For example RIP will preferred the second path.
    - R4 > R3 > R2 > R1 (100 Mbps FastEthernet links)
    - R4 > R5 > R1 (10 Mbps Ethernet links)
  - RIP is typically used only in small or test environments.
- **IS-IS**
  - Uses "Cost" as the metric, but it is not automatically derived from interface bandwidth. All links have an equal cost by default.
  - Can manually configure the cost of links to manipulate the path.
  - If the link costs are not configured will use the lowest hop count instead.
  - It is typically used in Service Provider networks or large organizations with their own MPLS network because of its scalability.
- **OSPF**
  - Uses "Cost" as the metric.
  - It takes bandwidth into account.
  - Can manually configure the cost of links to manipulate the path.
  - It is the most commonly deployed IGP today.
  - It is an open standard and supported by all vendor's routers.
  - It is complicated to maintain.
- **EIGRP**
  - Uses the bandwidth and delay of links to calculate the metric.
  - Load and reliability can also be considered but are ignored by default.
  - Can manually configure the delay on links to manipulate the path.
  - It started as Cisco proprietary but is now an open standard.
  - It is only supported on Cisco routers.
  - It is easy to maintain.
## ECMP (Equal Cost Multi Path)
- If multiple paths to a destination have an equal metric, they will be added to the routing table.
- The router will load balance the outbound traffic to the destination over the different paths.
- All IGP will perform ECMP by default.
- EIGRP is the only routing protocol that can do UnEqual Cost Multi Path. It must be manually configured.
## AD (Administrative Distance)
- It is used to choose between multiple paths learned from different routing protocols.
- Metric is used to choose between multiple paths learned from the same routing protocols.
- It is measured of how trusted the routing protocol is.
- If routes to the same destination are received via different routing protocols, the protocol with the best (lowest) AD wins.
- AD is considered first to narrow the choice down to the single best routing protocol.
- Metric is then considered to choose the best path or paths which go into the routing table.
- Example [AD/Metric]
  - `C 10.0.0.0/24 is directly connected, FastEthernet0/0`
  - `R 10.1.1.0/24 [120/2] via 10.0.0.2, 00:00:00, FastEthernet0/0`
- Default AD

| Route Source | Default AD |
| --- | --- |
| Connected Interface | 0 |
| Static Route | 1 |
| External BGP | 20 |
| EIGRP | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |

## Floating Static Routes
- When the best path to a destination is lost, it will be removed from the routing table and replaced with the next best path.
- If we want to configure a static route as a backup, the problem is that static routes have an AD of 1 which will always be preferred over the route learned via an IGP.
- Floating static route can be used to create a backup route.
- Floating static route for OSPF example.
  - `ip route 10.0.1.0 255.255.255.0 10.1.3.2 115`
  - By setting AD to be 115, greater than OSPF (110), the route will act as the backup route.
- Floating static route can also be used where we are using only static routing.
  - Without 5, the router will load balance between these two routes. Now 10.1.3.2 is the second to choose.
    - `ip route 10.0.1.0 255.255.255.0 10.1.1.2`
    - `ip route 10.0.1.0 255.255.255.0 10.1.3.2 5`

## Loopback Interfaces
- They are logical interfaces.
- They allow you to assign an IP address to a router or L3 switch, which is not tied to a physical interface. 
- They are usually assigned a /32 subnet mask to avoid wasting IP addresss.
- Loopback interfaces is enable by default and it never goes down.
- It is best practice to assign a loopback interface to your routers or L3 switches.
- It is commonly used for traffic that terminates on the router itself i.e. management traffic, Voice over IP, BGP peering etc.
- This provide redundancy if there are multiple paths to the router.
- The loopback is also used to identify the router (Router ID) in OSPF. 
- Multiple loopbacks can be configured. This is not common and only done where another loopback is required for a special use case.
- To create a loopback interface.
  - `R1# conf t`
  - `R1(config)# interface loopback 0`
  - `R1(config-if)# ip address 192.168.1.1 255.255.255.255`
  - No need to enable with `no shut`.
  - Add to EIGRP
  - `R1(config-if)# router eigrp 100`
  - `R1(config-router)# network 192.168.1.1 0.0.0.0` <- wildcard mask (inverse of subnet mask)

## Adjacencies
- How routers establish neighbor adjacencies.
  - IGP routing protocols are configured under global configuration mode and then enabled on idividual interfaces.
  - When the routing protocol is enabled on an interface the router will look for other devices on the link which are also running the routing protocol.
  - The router is sending out and listening for hello packets.
  - When a matching peer is found, the routers form an adjacency with each other.
  - Then they exchange routing information.
- Modern routing protocols use multicast for the hello packets. It is more efficient than broadcast because only running the same routing protocol will process the packet.
- The router will not send a hello packet to interfaces that are not included in the routing protocol. 
- And it will not advertise subnets on those interfaces to its neighbor adjacencies. They will not learn about these subnets.
- Passive interfaces
  - Allow to include an IP subnet in the routing protocol without sending updates out of the interface.
  - The neighbors will learn routes to these subnets, but internal network information will not be sent.
  - It is best practice to configure loopback interfaces as passive interfaces. 
    - Because they are not physical interfaces, it is impossible to form an adjacency on them.
    - Making them passive means that it will be advertised by the routing protocol, but it will not waste resources sending out and listening for hello packets.
  - Passive interfaces are used on:
    - Loopback interfaces.
    - Physical interfaces where the device on the other side belongs to another organization. Or we do not want to send routing information out but we do want our internal devices to know about the link.

## Route Precedence
- The best route for a packet decision is based on:
  - Longest prefix (most specific)
  - AD (Administrative Distance)
  - Metric
- An example these routes are for the exact same network and prefix:
  - 192.168.0.0/24 via EIGRP from R2 <- the best route, lowest AD
  - 192.168.0.0/24 via OSPF from R3
  - Criteria for which is the best route:
    - AD, the lower the better. If there is a tie.
    - Metric, the lower the better.
    - If there are multiple routes with the same AD and Metric, they will all enter the routing table and the router will perform Equal Cost Load Balancing over them.    
- Another example.
  - 10.0.0.0/24 via RIP (AD 120) from R2, Metric 5
  - 10.0.0.0/24 via OSPF (AD 110) from R3, Metric 2
  - 10.0.0.0/24 via EIGRP (AD 90) from R4, Metric 3072 <- the best route, lowest Metric
  - 10.0.0.0/24 via EIGRP (AD 90) from R5, Metric 6144
- Another example.
  - 192.168.0.0/30 as a Connected Route (AD 0) on interface G0/4
  - 192.168.0.0/24 via RIP (AD 120) from R2, Metric 1
  - 192.168.0.0/16 via OSPF (AD 110) from R3, Metric 11
  - 192.168.0.0/26 via EIGRP (AD 90) from R4, Metric 3072
  - 192.168.0.0/28 via EIGRP (AD 90) from R4, Metric 3072
  - They all go into the routing table because they are different routes going to different destinations (different prefix).
  - They overlap but they are different routes.
    - 192.168.0.0/30 > 192.168.0.1 - 192.168.0.3
    - 192.168.0.0/28 > 192.168.0.1 - 192.168.0.15
    - 192.168.0.0/26 > 192.168.0.1 - 192.168.0.63
    - 192.168.0.0/24 > 192.168.0.1 - 192.168.0.255
    - 192.168.0.0/16 > 192.168.0.1 - 192.168.255.255
  - If a packet with destination address 192.168.0.2 matches these routes, the longest prefix route will be selected.
    - 192.168.0.0/30 as a Connected Route (AD 0) on interface G0/4 <- the best route, the longest prefix
    - 192.168.0.0/24 via RIP (AD 120) from R2, Metric 1
    - 192.168.0.0/16 via OSPF (AD 110) from R3, Metric 11
    - 192.168.0.0/26 via EIGRP (AD 90) from R4, Metric 3072
    - 192.168.0.0/28 via EIGRP (AD 90) from R4, Metric 3072
  - If a packet with destination address 192.168.0.120 matches these routes, the longest prefix route will be selected.
    - 192.168.0.0/24 via RIP (AD 120) from R2, Metric 1 <- the best route, the longest prefix
    - 192.168.0.0/16 via OSPF (AD 110) from R3, Metric 11