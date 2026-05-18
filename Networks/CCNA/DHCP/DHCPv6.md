# DHCPv6

## Overview
- Each IPv6 node on the network needs a globally unique address (GUA) to communicate outside its local segment. But where a node get such an address from? There are a few options:
  - **Manual assignment** - Every node can be configured with an IPv6 address manually by an administrator. It is not a scalable approach and is prone to human error.

  - **DHCPv6** - The most widely adopted protocol for dynamically assigning addresses to hosts. Requires a DHCP server on the network and additional configuration.

  - **SLAAC** (Stateless Address Autoconfiguration) -  It was designed to be a simpler and more straight-forward approach to IPv6 auto-addressing. In its current implementation as defined in RFC 4862, SLAAC does not provide DNS server addresses to hosts and that is why it is not widely adopted at the moment.

- Dynamic GUA Assignment
  - Stateless
    - SLAAC Only
    - SLAAC with DHCP server (Stateless DHCPv6)
  - Stateful
    - DHCPv6 Server (Stateful DHCPv6)

## Stateless and Stateful
- **Stateless**
  - There is no server keeps track of what addresses have been assigned and what addresses are still available for an assignment.

  - nodes are responsible to resolve any duplicated address conflicts following the logic:
    - Generate an IPv6 address.
    - Run the Duplicate Address Detection (DAD).
    - If the address happens to be in use, generate another one and run DAD again.
- **Stateful**
  - There is a server or other device that keeps track of the state of each address assignment.

  - It tracks the address pool availability and resolves duplicated address conflicts.

  -  It also logs every assignment and keeps track of the expiration times.

## How does SLAAC works?
- **Step 1:** The node configures itself with a link-local address
  - When an IPv6 node is connected to an IPv6 enabled network, the first thing it typically does is to auto-configure itself with a link-local address.

  - The purpose of this local address is to enable the node to communicate at Layer 3 with other IPv6 devices in the local segment.

  - The most widely adopted way of auto-configuring a link-local address is by combining the link-local prefix FE80::/64 and the EUI-64 interface identifier, generated from the interface's MAC address.

- **Step 2:** The node performs Duplicate Address Detection (DAD)
  - To make sure that the address is actually unique in the local segment.

  - Even though the chances of another node having the same exact address are very slim. It has to perform a process called Duplicate Address Detection (DAD).

## IPv6 GUA Assignment
- By default, an IPv6-enabled router periodically send ICMPv6 RAs which simplifies how a host can dynamically create or acquire its IPv6 configuration.

  - A host can dynamically be assigned a GUA using stateless and stateful services.

  - All stateless and stateful methods in this module use ICMPv6 RA messages to suggest to the host how to create or acquire its IPv6 configuration.

  - Although host operating systems follow the suggestion of the RA, the actual decision is ultimately up to the host

## Notes
- SLAAC
  - Stateless Address Auto-configuration

  - It is a mechanism that enables each host on the network to auto-configure a unique IPv6 address without any device keeping track of which address is assigned to which node.
    - SLAAC Only
    - SLAAC with DHCP server (Stateless DHCPv6)
- DHCPv6 Server (Stateful DHCPv6)

- EUI-64, an example of how a local address is generated from MAC address 7007.1234.5678.
  - `7007.1234.5678`
  - Insert 0xFFFE in the middle of the MAC address
    - `7007.12 FFFE 34.5678`
  - Flip the $7^{th}$ bit of the MAC address  (convert the first two hex digits to binary first)
    - `7007.12 FFFE 34.5678`
    - `7 0`
    - `0111 0000`
    - `0111 0010`
    - `7 2`
    - `7207.12 FFFE 34.5678`
  - Combine the link-local prefix with the EUI-64 identifier:
    - `FE80::7207:12FF:FE34:5678/64`

- DAD
  - DAD process is used by a host to ensure that the IPv6 GUA is unique.
  - The host sends an ICMPv6 NS message with a special type of address called solicited-node multicast address.
  - If no other devices respond with an NA message, then the address is virtually guaranteed to be unique and can be used by the host.

## Sources
- https://www.networkacademy.io/ccna/ipv6/stateless-address-autoconfiguration-slaac
- https://www.networkacademy.io/ccna/ipv6/ipv6-on-windows
- https://www.networkacademy.io/ccna/ipv6/stateless-dhcpv6
- https://www.networkacademy.io/ccna/ipv6/stateful-dhcpv6