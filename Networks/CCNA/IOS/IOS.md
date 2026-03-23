# IOS
* [Switch Setup](#switch-setup)
* [Switch Password Setup](#switch-password-setup)
* [Switch VLAN Setup](#switch-vlan-setup)
* [Switch Trunk Setup](#switch-trunk-setup)
* [Router Trunk Setup](#router-trunk-setup)

## Switch Setup

|Command|Description|
|---|---|
|`Switch> enable`|Enter Privileged EXEC mode.|
|`Switch# configure terminal`|Enter Global Config mode.|
|`Switch(config)# hostname S1`|Name the switch to "S1".|
|`S1(config)# exit`|Exit the current mode.|
|`S1(config)# end`|Exit to Privileged EXEC mode.|

## Switch Password Setup

|Command|Description|
|---|---|
|`S1> enable`|Enter Privileged EXEC mode.|
|`S1# configure terminal`|Enter Global Config mode.|
|`S1(config)# enable secret cisco`|Set Privileged EXEC mode password to "cisco".|
|`S1(config)# line console 0`|Enter line configuration mode.|
|`S1(config-line)# password class`|Set line console password to "class".|
|`S1(config-line)# login`|Enable line console login.|
|`S1(config-line)# exit`|Exit line conf. mode.|
|`S1(config)# service password-encryption`|Encrypt password.|

## Switch VLAN Setup

|Command|Description|
|---|---|
|`S1> enable`|Enter Privileged EXEC mode.|
|`S1# configure terminal`|Enter Global Config mode.|
|`S1(config)# vlan 10`|Create VLAN 10.|
|`S1(config-vlan)# name Management`|Name VLAN 10 to "Management".|
|`S1(config)# vlan 20`|Create VLAN 20.|
|`S1(config-vlan)# name Marketing`|Name VLAN 20 to "Marketing".|
|`S1(config)# vlan 99`|Create VLAN 99.|
|`S1(config-vlan)# name NATIVE-UNUSED`|Name VLAN 99 to "NATIVE-UNUSED".|
|`S1(config-vlan)# exit`|Exit VLAN conf. mode.|
|`S1(config)# interface range f0/1-4`|Enter interface conf. mode for f0/1 - f0/4.|
|`S1(config-if-range)# switchport mode access`|Enable access mode.|
|`S1(config-if-range)# switchport access vlan 10`|Enable access to VLAN 10.|
|`S1(config-if-range)# exit`|Exit to Global conf. mode.|
|`S1(config)# interface range f0/5-24`|Enter interface conf. mode for f0/5 - f0/24.|
|`S1(config-if-range)# switchport mode access`|Enable access mode.|
|`S1(config-if-range)# switchport access vlan 99`|Enable access to VLAN 99.|
|`S1(config-if-range)# exit`|Exit VLAN conf. mode.|
|`S1(config)# no vlan 20`|Remove VLAN 20.|
|`S1(config)# end`|Exit to Privileged EXEC mode.|
|`S1# show vlan brief`|Show VLAN.|

## Switch Trunk Setup

|Command|Description|
|---|---|
|`S1> enable`|Enter Privileged EXEC mode.|
|`S1# configure terminal`|Enter Global Config mode.|
|`S1(config)# interface g0/1`|Enter interface conf. mode.|
|`S1(config-if)# switchport mode trunk`|Enable trunk mode.|
|`S1(config-if)# switchport trunk native vlan 99`|Set trunk native to VLAN 99.|
|`S1(config-if)# switchport trunk allowed vlan 10,20,99`|Set trunk to allow VLAN 10, 20, 99.|
|`S1(config-if)# no shutdown`|Enable the interface.|
|`S1(config-if)# exit`|Exit interface conf. mode.|

## Router Trunk Setup

|Command|Description|
|---|---|
|`R1> enable`|Enter Privileged EXEC mode.|
|`R1# configure terminal`|Enter Global Config mode.|
|`R1(config)# interface g0/0.10`|Enter and create subinterface conf. mode.|
|`R1(config-subif)# description Default Gateway for VLAN 10`|Create a description for the subinterface.|
|`R1(config-subif)# encapsulation dot1q 10`|Set encapsulation type.|
|`R1(config-subif)# ip address 192.168.10.1 255.255.255.0`|Set IPv4 address.|
|`R1(config)# interface g0/0`|Enter interface conf. mode.|
|`R1(config-if)# description Trunk link to Sw-A`|Create a description.|
|`R1(config-if)# no shutdown`|Enable the interface.|
|`R1(config-if)# end`|Exit to Privileged EXEC mode.|