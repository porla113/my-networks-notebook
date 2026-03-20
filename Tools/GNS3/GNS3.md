# GNS3
## GNS3 Installation (20.03.2026)
- download install GNS3 version 2.2.56.1 (for Windows)
- Error during installation, cloud not download and install Wireshark
- Error during installation, cloud not download and install Solar-Putty
- Download and install Wireshark, adjust the Wireshark path in a reader command (Edit > Preferences > Packet capture > Packet capture reader command).
- Download PuTTY (not necessary GNS3 already has putty_standalone in its installed folder)
- Creating a switch produces error "wpcap.dll was not found". Reinstall Npcap with "Install Npcap in WinPcap API-compatible Mode" option checked.

## General
### Show grid and snap to grid (20.03.2026)
- View > show the grid
- View > snap to grid

### Change PuTTY font size (20.03.2026)
- Click the top left icon.
- Change Settings... > Window > Appearance Font settings > Change...
- After font size modifing, go to Session > select Default Settings > Save
- Click Apply

### Config VPCS (Virtual PC Simulator)
|Command|Description|
|---|---|
|`?`|Show help.|
|`ip 10.1.1.1 255.255.255.0`|Set IP.|
|`ping 10.1.1.2`|Set IP.|
|`save`|Save configuration.|