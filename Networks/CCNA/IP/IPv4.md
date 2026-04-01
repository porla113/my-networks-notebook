# IPv4
## Network and Host Portions
**Example**\
IPv4 address: `192.168.10.10`\
Subnet mask: `255.255.255.0`\
Network portion: `192.168.10`\
Host portion: `.10`

## Private Addresses
- For hosts in a closed private network (not routable).
- `10.0.0.0` - `10.255.255.255`
- `172.16.0.0` - `172.31.255.255`
- `192.168.0.0` - `192.168.255.255`

## Class
|Class|First Octet|Default Subnet Mask|
|---|---|---|
|A|1 - 126|255.0.0.0|
|B|128 - 191|255.255.0.0|
|C|192 - 223|255.255.255.0|
|D|224 - 239||
|E|240 - 255||
### Class A
- For very large networks.
- The high-order bit (the left most bit) is always set to 0.
- The default subnet mask is /8.
- Allow for 126 networks and 16,777,214 hosts per network.
- Valid netwroks range from `1.0.0.0` - `126.0.0.0/8`
- `127.0.0.0/8` is reserved for the loopback address for testing the local computer.
- `127.0.0.1` - `127.255.255.255` is not for host addresses

### Class B
- For medium- to large-sized networks.
- The two high-order bits are always set to `1 0`.
- The default subnet mask is /16.
- Allow for 16,384 networks and 65,534 hosts per network.
- Valid networks range from `128.0.0.0` to `191.255.0.0/16`

### Class C
- For small networks.
- The three high-order bits are always set to `1 1 0`.
- The default subnet mask is /24.
- Allow for 2,097,152 networks and 254 hosts per network.
- Valid networks range from `192.0.0.0` to `223.255.255.0/24`

### Class D
- Reserved for IP multicast addresses.
- The four high-order bits are always set to `1 1 1 0`.
- No default subnet mask.
- Valid addresses range from `224.0.0.0` to `239.255.255.255`

### Class E
- Are experimental and reserved for future use.
- The four high-order bits are always set to `1 1 1 1`.
- No default subnet mask.
- Valid addresses range from `240.0.0.0` to `255.255.255.255`
- `255.255.255.255` is the broadcast address for "this network".

## CIDR
- Classless Inter-Domain Routing
- Remove the fixed /8, /16, /24 (Class A, B, C).
- Allow to be subnetted into smaller networks.
- Route summarisation benefits
  - ISP A has allocated address blocks to their customers `175.10.0.0/24`, `175.10.1.0/24`, ..., `175.10.255.0/24`
  - ISP B has allocated address blocks to their customers `175.11.0.0/24`, `175.11.1.0/24`, ..., `175.11.255.0/24`
  - ISP A only advertises `175.10.0.0/16` and learns `175.11.0.0/16`
  - ISP B only learns `175.10.0.0/16` and advertises `175.11.0.0/16` 

## Subnetting
- We need to borrow host bits and add to network portion.
- $2^n$ = avialable subnets 
  - $n$ = number of borrowed bits.
- $2^n - 2$ = avialable hosts
  - $n$ = number of host bits
  - Exclude network and broadcast address.

|$2^0$|$2^1$|$2^2$|$2^3$|$2^4$|$2^5$|$2^6$|$2^7$|$2^8$|$2^9$|$2^{10}$|$2^{11}$|$2^{12}$|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|1|2|4|8|16|32|64|128|256|512|1024|2048|4096|

**Example 1**
- We have been allocated Class C `200.15.10.0/24`
  - `11001000.00001111.00001010.00000000`
  - `11111111.11111111.11111111.00000000`
- We are using /31.
  - `11001000.00001111.00001010.00000000`
  - `11111111.11111111.11111111.11111110`
  - $2^7 = 128$ subnets
  - $2^1 = 2$ hosts each
  - Valid host addresses:
    - `200.15.10.0` - `200.15.10.1` 
    - `200.15.10.2` - `200.15.10.3` 
    - `200.15.10.4` - `200.15.10.5` 
    - to 
    - `200.15.10.252` - `200.15.10.253` 
    - `200.15.10.254` - `200.15.10.255` 
  -  **Note:** /31 is supported on Cisco routers for point to point links which have no need for a network or broadcast address.

**Example 2**
- We have been allocated Class C `200.15.10.0/24`
  - `11001000.00001111.00001010.00000000`
  - `11111111.11111111.11111111.00000000`
- We are using /30.
  - `11001000.00001111.00001010.00000000`
  - `11111111.11111111.11111111.11111100`
  - $2^6 = 64$ subnets
  - $2^2-2 = 2$ avialable hosts each (exc. network and broadcast address)
  - Valid host addresses:
    - `200.15.10.0` - `200.15.10.3` (netw .0, broadc .3)
    - `200.15.10.4` - `200.15.10.7` (netw .4, broadc .7)
    - `200.15.10.8` - `200.15.10.11` (netw .8, broadc .11)
    - to 
    - `200.15.10.248` - `200.15.10.251` (netw .248, broadc .251)
    - `200.15.10.252` - `200.15.10.255` (netw .252, broadc .255)

**Example 3**
- We have been allocated Class C `200.15.10.0/24`
  - `11001000.00001111.00001010.00000000`
  - `11111111.11111111.11111111.00000000`
- We are using /29.
  - `11001000.00001111.00001010.00000000`
  - `11111111.11111111.11111111.11111000`
  - $2^5 = 32$ subnets
  - $2^3-2 = 6$ avialable hosts each (exc. network and broadcast address)
  - Valid host addresses:
    - `200.15.10.0` - `200.15.10.7` (netw .0, broadc .7)
    - `200.15.10.8` - `200.15.10.15` (netw .8, broadc .15)
    - `200.15.10.16` - `200.15.10.23` (netw .16, broadc .23)
    - to 
    - `200.15.10.240` - `200.15.10.247` (netw .240, broadc .247)
    - `200.15.10.248` - `200.15.10.255` (netw .248, broadc .255)

**Example 4**
  - /28 = 16 subnets of 14 available hosts each
  - /27 = 8 subnets of 30 available hosts each
  - /26 = 4 subnets of 62 available hosts each
  - /25 = 2 subnets of 126 available hosts each

### VLSM
  - Variable Length Subnet Masks
  - Allow to size subnets differently.