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
- `--bssid` : Select MAC adresse to sniff
  - usage
    - ```bash
      airodump-ng --bssid <MAC_ADDRESS> --channel <CHANNEL_ID> <INTERFACE>`
      ``` 
- `--channel`: Select channel 
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



