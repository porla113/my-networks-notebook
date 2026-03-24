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
