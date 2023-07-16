# Network Hacking Main Page

## Show available INTERFACE info
```bash
ifconfig
iwconfig
```

## Changing MAC adresse
```bash
ifconfig <INTERFACE> down
ifconfig <INTERFACE> hw ether <MAC_ADDRESS> # hw for hardware
ifconfig <INTERFACE> up
```

## WIFI Sniffing

`aerodump-ng` is a packet sniffer
 - Use to capture packets within range
 - Display detailed info about networks
 - part of `aircrak-ng` suit

usage : `airodump-ng <INTERFACE>`

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



