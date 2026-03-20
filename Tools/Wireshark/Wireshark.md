# Wireshark

## Configuring

### Profile (20.03.2026)
The default is Default profile. It is at the bottom right of the window. Click to select a profile.

#### Create a new profile (20.03.2026)
- Right click and select New to create a new profile (also import and export). Type a name and click OK.
- Profiles will be saved automatically.

#### Adjust the layout (20.03.2026)
- The biggest pane is the Packet List pane.
- The lower left pane is the Packet Detail pane.
- The lower right pane is the Packet Bytes pane.
- Go to Edit > Preferences > Layout
- Select a layout, click Apply, and click OK.

#### Change time display format (20.03.2026)
- View > Time Display Format > Seconds Since Previous Displayed Packet (equivalent to delta time)
- View > Time Display Format > Microseconds (Nanoseconds is default which is too much).
- The default is Seconds Since First Captured Packet

#### Add a new column (20.03.2026)
- Go to Edit > Preferences > Appearance > Columns
- Click + to add a new column
- Name the column (ex. Delta)
- Select a type (ex. Delta time displayed)
- Click and drag to rearrange the order (ex. I put it under the Time column).
- Click Apply.

#### Add a new column from a packet field (20.03.2026)
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

#### Reset the Default profile (20.03.2026)
- Right click the profile > Manage Profiles.
- Select Default and click - to remove it.
- Close Wireshark and re-open it. Wireshark will create a brand new Default profile.

#### Adjust traffic Color (20.03.2026)
- This is an example of coloring the TCP SYN packet. We need two pieces of information.
  1. The (display) filter for that packet, select the field (ex. flags-SYN) and drag it to the filter field (above the column header). The filter will be `tcp.flags.syn == True`
  2. The coloring rule they belong to, select a packet and go to the first line in the Packet Details pane. Expand it and look for Coloring Rule Name metadata.
- Go to View > Coloring Rules
- They are stacking rules. If a packet matches more than one rule, the top filter is the one that applies.
- We can add or delete rule by clicking + / -
- Remove the “TCP SYN/FIN” rule and add a new rule, “TCP-SYN” (conversation start). The filter is tcp.flags.syn == 1. Change the background color to bright green.
- Move it down below “ICMP errors”. Click OK.
- Add another rule, “TCP-FIN” (conversation stop). The filter is tcp.flags.fin == 1. Change the background color to bright blue and move it below the TCP-SYN rule. Click OK.
- They will be standout especially when looking in the mini scroll bar.
