# DHCPv4
## Overview
- In IP environment, before a computer can communicate to another one, they need to have their own IP addresses. There are two ways of configuring an IP address on a device:
  - Statically assign an IP address. This means we manually type an IP address for this computer.

  - Use a protocol so that the computer can obtain its IP address automatically (dynamically).

- The most popular protocol nowadays to do this task is called Dynamic Host Configuration Protocol (DHCP).

- An advantage of using DHCP is to facilitate the network administrator, do not have to configure IP addresses for every device on the network.

- Another big advantage of using DHCP is the ability to join a network without knowing detail about it. 
  - For example you go to a coffee shop, with DHCP enabled on your computer, you can go online without doing anything.

  - Next day you go online at your school and you don’t have to configure anything either even though the networks of the coffee shop and your school are different (for example, the network of the coffee shop is 192.168.1.0/24 while that of your company is 10.0.0.0/8). 

  - Without DHCP, you have to ask someone who knows about the networks at your location then manually choosing an IP address in that range. In bad situation, your chosen IP can be same as someone else who is also using that network and an address conflict may occur.

## Concepts
- DHCPv4 assigns IPv4 addresses and other network configuration information dynamically.

- A dedicated DHCPv4 server is scalable and relatively easy to manage. However, in a small branch or SOHO location, a Cisco router can be configured to provide DHCPv4 services

- The DHCPv4 server dynamically assigns, or leases, an IPv4 address from a pool of addresses for a limited period of time chosen by the server, or until the client no longer needs the address.

- Clients lease the information from the server for an administratively defined period (typically anywhere from 24 hours to a week or more). When the lease nearly expires, the client must renew a lease (usually 30 minutes before expire) which is typically get the same address.

## DHCPv4 Operation
- DHCPv4 works in a client/server mode.

- When a client communicates with a DHCPv4 server, the server assigns or leases an IPv4 address to that client.

- The client connects to the network with that leased IPv4 address until the lease expires.

- The client must contact the DHCP server periodically to extend the lease.

- This lease mechanism ensures that clients that move or power off do not keep addresses that they no longer need.

- When a lease expires, the DHCP server returns the address to the pool where it can be reallocated as necessary.

## Steps to Obtain a Lease

When the client boots / join a network, it begins a four-step process to obtain a lease:

- DHCP Discover (DHCPDISCOVER)
- DHCP Offer (DHCPOFFER)
- DHCP Request (DHCPREQUEST)
- DHCP Acknowledgment (DHCPACK)

### DHCPDISCOVER
- The client starts the process using a broadcast DHCPDISCOVER message with its own MAC address.

- Because the client still does not know the subnet which it belongs (it can not use subnet broadcast), the DHCPDISCOVER is an all-subnets broadcast / local broadcast (destination IP address of 255.255.255.255, which is a layer 3 broadcast address) and a destination MAC address of FF-FF-FF-FF-FF-FF (which is a layer 2 broadcast address).

- The purpose is to find DHCPv4 server on the network.

### DHCPOFFER
- After receiving the discover message, the DHCP Server will dynamically pick up an unassigned IP address from its IP pool and unicast* a DHCPOFFER message to the client.
  - In fact, the DHCPOFFER is a layer 3 broadcast message (the IP destination is 255.255.255.255) but a layer 2 unicast message (the MAC destination is the MAC of the DHCP Client, not FF-FF-FF-FF-FF-FF). So in some books they may say it is a broadcast or unicast message.

### DHCPREQUEST
- If the client accepts the offer, it then broadcasts a DHCPREQUEST message saying it will take this IP address.

- It is called request message because the client might deny the offer by requesting another IP address. 

- Notice that DHCPREQUEST message is still a broadcast message because the DHCP client has still not received an acknowledged IP. 

- Also a DHCP Client can receive DHCPOFFER messages from other DHCP Servers so sending broadcast DHCPREQUEST message is also a way to inform other offers have been rejected.

### DHCPACK
- When the DHCP Server receives the DHCPREQUEST message from the client, the DHCP Server accepts the request by sending the client a unicast DHCPACK (acknowledgement) message.

## Notes
- DHCP operates at the Application Layer (Layer 7) of the OSI model.

- Port 67: DHCP/BOOTP server

- Port 68: DHCP/BOOTP client

- Two-step renew process:
  - DHCPREQUEST
    - Before the lease expires, the client sends a DHCPREQUEST message directly to the DHCPv4 server that originally offered the IPv4 address.

    - If a DHCPACK is not received within a specified amount of time, the client broadcasts another DHCPREQUEST so that one of the other DHCPv4 servers can extend the lease.

  - DHCPACK
    - On receiving the DHCPREQUEST message, the server verifies the lease information by returning a DHCPACK.

- DHCPv4 relay
  - If the DHCP Server is not on the same subnet with the DHCP Client, we need to configure the router on the DHCP client side to act as a DHCP Relay Agent so that it can forward DHCP messages between the DHCP Client & DHCP Server.

  - To make a router a DHCP Relay Agent, simply put the “ip helper-address <IP-address-of-DHCP-Server>” command under the interface that receives the DHCP messages from the DHCP Client.
```
R1(config)# interface f0/0
R1(config-if)# ip helper-address 192.168.11.6
R1(config-if)# end
```
  - As we know, router does not forward broadcast packets (it drops them instead) so DHCP messages like DHCPDISCOVER message will be dropped. But with the “ip helper-address …” command, the router will accept that broadcast message and covert it into a unicast packet and forward it to the DHCP Server.

    - If there are other routers on the DHCP path then only the first Layer 3 interface that receives the DHCP request from the DHCP client needs the “ip helper-address …” command. 

    - We do not need to configure this command on other routers in the path because only the first DHCP relay agent needs to convert the DHCPDISCOVER message from broadcast to unicast. Other routers between the relay agent and the DHCP server simply route the unicast packet normally.

- When a DHCP address conflict occurs.
  - During the IP assignment process, the DHCP Server uses ping to test the availability of an IP before issuing it to the client. 

  - If no one replies then the DHCP Server believes that IP has not been allocated and it can safely assign that IP to a client.

  - If someone answers the ping, the DHCP Server records a conflict, the address is then removed from the DHCP pool and it will not be assigned to a client until the administrator resolves the conflict manually.


## Resources
- https://www.9tut.com/dhcp-tutorial