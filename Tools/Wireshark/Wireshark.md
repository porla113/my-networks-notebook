# Wireshark
* ### Configuring
  * [Profile](#profile)
     * [Creating a new profile](#creating-a-new-profile)
     * [Adjusting the layout](#adjusting-the-layout)
     * [Changing time display format](#changing-time-display-format)
     * [Adding a new column](#adding-a-new-column)
     * [Adding a new column from a packet field](#adding-a-new-column-from-a-packet-field)
     * [Resetting the Default profile](#resetting-the-default-profile)
     * [Adjusting traffic Color](#adjusting-traffic-color)
* ### [Filters](#filters-1)
  * [Common filters](#common-filters)
  * [Operators in filters](#operators-in-filters)
  * [Special filters](#special-filters)
  * [Creating a filter](#creating-a-filter)
  * [Filter examples](#filter-examples)
* ### [Capturing Packet](#capturing-packet-1)
  * [Before capturing](#before-capturing)
  * [Capturing packets](#capturing-packets)
  * [Capturing with Wireshark interface](#capturing-with-wireshark-interface)

## Configuring

### Profile 
(20.03.2026)\
The default is Default profile. It is at the bottom right of the window. Click to select a profile.

#### Creating a new profile
(20.03.2026)
- Right click and select New to create a new profile (also import and export). Type a name and click OK.
- Profiles will be saved automatically.

#### Adjusting the layout
(20.03.2026)
- The biggest pane is the Packet List pane.
- The lower left pane is the Packet Detail pane.
- The lower right pane is the Packet Bytes pane.
- Go to Edit > Preferences > Layout
- Select a layout, click Apply, and click OK.

#### Changing time display format
(20.03.2026)
- View > Time Display Format > Seconds Since Previous Displayed Packet (equivalent to delta time)
- View > Time Display Format > Microseconds (Nanoseconds is default which is too much).
- The default is Seconds Since First Captured Packet

#### Adding a new column
(20.03.2026)
- Go to Edit > Preferences > Appearance > Columns
- Click + to add a new column
- Name the column (ex. Delta)
- Select a type (ex. Delta time displayed)
- Click and drag to rearrange the order (ex. I put it under the Time column).
- Click Apply.

#### Adding a new column from a packet field
(20.03.2026)
- Select a packet 
- For example, select a packet (ex. DNS).
1. Method 1:
   - Select a field (ex. source port). Right click and select “Apply as Column”
2. Method 2:
   - Click and drag the field (ex. destination port) to where you want.
- However, the columns only display UDP source and destination port, there is no display for TCP. It is because we created the columns from the DNS packet which is UDP.
- Select the source port from a TCP packet and look to the bottom left. There is a field name, tcp.srcport.
- Right click the column header (ex. Source Port) and select Edit Column. The option will appear above the column header. 
- At the Fields, add “upd.srcport or tcp.srcport” (also can rename the title here). Click OK.

#### Resetting the Default profile
(20.03.2026)
- Right click the profile > Manage Profiles.
- Select Default and click - to remove it.
- Close Wireshark and re-open it. Wireshark will create a brand new Default profile.

#### Adjusting traffic Color
(20.03.2026)
- This is an example of coloring the TCP SYN packet. We need two pieces of information.
  1. The (display) filter for that packet, select the field (ex. flags-SYN) and drag it to the filter field (above the column header). The filter will be `tcp.flags.syn == True`
  2. The coloring rule they belong to, select a packet and go to the first line in the Packet Details pane. Expand it and look for Coloring Rule Name metadata.
- Go to View > Coloring Rules
- They are stacking rules. If a packet matches more than one rule, the top filter is the one that applies.
- We can add or delete rule by clicking + / -
- Remove the “TCP SYN/FIN” rule and add a new rule, “TCP-SYN” (conversation start). The filter is `tcp.flags.syn == 1`. Change the background color to bright green.
- Move it down below “ICMP errors”. Click OK.
- Add another rule, “TCP-FIN” (conversation stop). The filter is `tcp.flags.fin == 1`. Change the background color to bright blue and move it below the TCP-SYN rule. Click OK.
- They will be standout especially when looking in the mini scroll bar.

#### Adding Filer Buttons
(24.03.2026)
- Make sure you are in the right profile.
- Type a display filter, then go to the + (the right most) and click to create a button.
- Give it a label name / comment.
- Click Ok.
- Right click at a button to edit or go to Edit > Preferences > Filter Buttons.


## Filters
(24.03.2026)
- There are two types of filter in Wireshark, capture and display filter.
- When a packet arrives at NIC the capture filter will be applied first and keeps it in the buffer. To adjust the capture options, click the fourth icon (black target icon) to the right of the Start Capturing icon (shark fin icon).
- Wireshark brings those packets in the buffer and shows them to the user. The user will apply the display filter to analyze the packets.
- Advice is to keep the capture filter simple (or leave it blank). Capture everything and use a display filter.

### Common filters 
(24.03.2026)
|Filter Type|Display Filter|
|---|---|
|IPv4 Address|`ip.addr==10.0.0.1`|
|IPv4 Source|`ip.src==10.0.0.1`|
|IPv4 Range|`ip.addr==10.0.0.0/24`|
|TCP Port|`tcp.port==80`|
|TCP SYNs|`tcp.flags.syn==1`|

### Operators in filters 
(24.03.2026)
|Symbol|Equivalent|
|---|---|
| == | eq |
| ! | not |
| \|\| | or |
| && | and |
| > | gt |
| < | lt |

- Ex. ip.addr == 192.168.1.1 && tcp
- Ex. ip.addr eq 192.168.1.1 and tcp
- Ex. ip.addr eq 192.168.1.1 && tcp

### Special filters 
(24.03.2026)

| Keyword | Example | Note |
|---|---|---|
| **contains** (exact string) | frame **contains** “google” | Case sensitive |
| **matches** (regex) | http.host **matches** “\\.(org|com|net)” | Case insensitive |
| **in** {range} | tcp.port **in** {80,443,8000..8004} |  |

### Creating a filter
(24.03.2026)
- There are several ways to create a filter.
  1. From the packet list pane, click the interested field and drag to the filter. Wireshark will automatically convert into a filter, we can then modify it later.
  2. Right click the interested packet > Conversation FIlter > IPv4. A filter for both IP addresses (source and destination) is created for you.

### Filter examples
(24.03.2026)
- `!ip.addr == 192.168.4.1` - A filter to show every packet, but not packets from a specific address.
- `!tcp.port in {80,443}` - A filter to show every packet, but not TCP packets from port 80 and 443.
- `tcp contains “google”` - A filter to show every TCP packet that has a “google” in it (info).
- `frame contains “google”` - A filter to show every packet (regardless of protocol) that has a “google” in it (info).
- To set up TCP conversation filter, right click on the interested packet > Conversation Filter > TCP

## Capturing Packet
(25.03.2026)

### Before capturing
(25.03.2026)
- Plan before capturing. Don’t run into a data center and grab a ton of traffic.
- Some questions to think about the problem:
  - Who is being impacted?
  - Does the problem ever go away?
  - Can it be reproduced on demand?
- Some questions to think about capturing the problem:
  - What capture methods do we have at our disposal?
  - Can we capture the client side?
  - Can we capture the server side?
  - Can we capture somewhere in the middle?
  - Can we capture all three?
- Some questions to think about capturing method:
  - What type of capture equipment do we have?
  - Do we just have a laptop?
  - Do we have some stronger capture gear?
  - Do we have some real hardware?

### Capturing packets
(25.03.2026)
- There are several ways to capture packets:
  - Install a tap.
    - Have a cost.
  - Configure SPAN (Switch Port Analyzer) or port mirroring on the switch.
    - Tell the switch to send a copy of traffic on the specific ports to the SPAN port.
    - It is free, just have to configure.
    - If over-provision these ports, more traffic will be sent to the port than it can handle.
  - Install Wireshark on the client side endpoint.
    - It is free, quick, and dirty.
    - Might not fully see how that traffic looks on the wire.
  - Use capability built in of network infrastructure devices (router, firewall, or loadbalancer).
    - It is a feature that we can use. 
    - It is free, quick.
    - We are putting another load on top of already busy devices.
    - These devices do not have very large buffers.
  - Use available features of cloud services.
    - Depends on the support package.
    - Check what kind of cloud-based capture we have.
  - Install Wireshark on the server side endpoint.
    - We are putting another load on the server.
- The best case would be able to capture on the client side and the server side at the same time.
- If possible try to capture the traffic from more than one vantage point.
- If we can only get one vantage point, then that is all we got.
- Should we use a capture filter?
  - If we capture on the client side, we might not. Capture everything, filter later.
  - If we capture on the server side and we want to capture just packets from a specific client, we might need to. Because the server gets traffic from many clients. (Warning!!! We will miss conversations between the server and the backend system.)

### Capturing with Wireshark interface
(25.03.2026)
- Open Wireshark, and double click the interface you want to capture.
- Wireshark is going to keep capturing packets until we press the stop button or the hard drive is full or the system crashes.
- We can configure how Wireshark saves packets to disk.
  - Go to Capture Options > Output > Browse… (location to save the file)
  - Set Output format to “pcapng” (new version, new features i.e. comments)
  - Check “Create a new file automatically”. Check an option (after 100 megabytes for example).
  - Check Use a ring buffer with 20 files.
- When the problem strikes, press stop and note down the time. Analyze the packet capture that was running at the time and also the one or two before it.
