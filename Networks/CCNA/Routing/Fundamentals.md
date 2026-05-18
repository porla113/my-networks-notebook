# Fundamentals
## Router Functions
- Determinig the best path to available networks.
- Forwarding traffic to those networks.
## Routing Table
- It contains paths to a network used for forwarding traffic.
- It consists of directly connected networks and routes configured by the admin or dynamically learned through a routing protocol.
## Route
- ### Connected route
  - As soon as an admin configure IP addresses on the router's interfaces and turn on (`no shutdown`).
  - They are networks that are physically attaced to the router's active interfaces.
- ### Local route
  - It is the exact IP address of a router interface.
  - Always have a /32 prefix.
  - Introduced in Cisco IOS 15+.
- ### Static route
  - They are routes to networks that are not directly attached to the router.
  - An admin can manually add them to the routing table, or the router can learn via a routing protocol. 
- ### Default route
  - It is a static route that acts as a "Gateway of Last Resort".
  - If a router receives a packet destined for a network that is not in its routing table, it matches the default route.
  - The address is `0.0.0.0 0.0.0.0` for IPv4 and `::/0` for IPv6
- ### Summary route
  - It leads to less memory usage in a router as its routing table contain less routes.
  - It also leads to less CPU usage as changes in the network only affect other routers in the same area.
  - Example:
    - `10.1.0.0 255.255.255.0 10.0.0.2`
    - `10.1.1.0 255.255.255.0 10.0.0.1`
    - `10.1.2.0 255.255.255.0 10.0.0.1`
    - Instead of having 3 routes: `10.1.0.0 255.255.0.0 10.0.0.2`
    - It doesn't have to be classful boundaries. To summarise the range 10.1.0.0 to 10.1.3.0: `10.1.0.0 255.255.252.0 10.0.0.2`
- ### Longest prefix match
  - When there are overlapping routes, the longest prefix will be picked.
  - `ip route 10.1.0.0 255.255.0.0 10.0.0.2`
  - `ip route 10.1.3.0 255.255.255.0 10.0.3.2` <- selected
- ### Load balancing
  - When there are multiple routes to the same network, the router will load balance between them.
  - `ip route 10.1.0.0 255.255.0.0 10.0.0.2`
  - `ip route 10.1.0.0 255.255.0.0 10.0.3.2`
