# IOS
* [Switch Setup](#switch-setup)
* [Switch Password Setup](#switch-password-setup)

## Switch Setup

|Command|Description|
|---|---|
|`Switch> enable`|Enter Privileged EXEC mode.|
|`Switch# configure terminal`|Enter Global Config mode.|
|`Switch(config)# hostname S1`|Name the switch to "S1".|
|`S1(config)# exit`|Exit Global Config mode.|
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
|`S1(config)# exit`|Exit Global Config mode.|
|`S1(config)# end`|Exit to Privileged EXEC mode.|