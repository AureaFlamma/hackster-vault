# WIFI 

## WIFI Basics

- A router has several physical *radio chips* (aka radios, transmitters). Mine has 2 - a 2.4Ghz one and a 5Ghz one.
- Each radio has a software *access point* role. Some radios can have more than one AP, e.g. a normal wifi, a guest wifi, an IoT wifi.
  Analogy: This is to the radio what disk partitions are to a hard drive.
- Each thing connected to WiFi has a *MAC address* (Media Access Control).
- The MAC of an AP is called a *BSSID* - Basic Service Set Identifier
- In addition, a wifi router has a *SSID* - Service Set Identifier. This is the human readable label which I see on the wifi list.

So, each router has one SSID, with several radios, each of the radio having at least one AP with one MAC address/BSSID

- *Roaming* is when a receiver device moves from one AP to another on the same network (different BSSIDs, same SSID), as a result of,
  e.g. moving around a building.
- *Band steering* is when router forces roaming even when the client device doesn't move - for example for load balancing or to 
  get the client on a "better" band.


## WIFI Band steering problem - steps to solve
My problem was that there were hiccups during connect/disconnect. So every time my router band steered me between the bands,
there would be interruption to service. It was fixed in the *Network Manager*, by forcing my laptop to stay on one of the two
AP's.


### Step 0 — Find your router’s BSSIDs

While connected to home WiFi:

nmcli device wifi list

Or:

iw dev wlan0 scan | grep -E 'SSID|BSS '

Note:
• 2.4 GHz AP → use for bssid lock (yours was A2:B5:3C:BA:C8:6E)
• 5 GHz AP → use for denylist (yours was A2:B5:3C:BA:D8:76)

Replace these if your router changes or after a factory reset.

### Step 1 — Apply the band locks (GUI or terminal) - MAIN FIx

Terminal (replace connection name if different):
```
sudo nmcli connection modify "VodafoneBAC866" \
802-11-wireless.band bg \
802-11-wireless.bssid A2:B5:3C:BA:C8:6E \
802-11-wireless.mac-address-denylist "A2:B5:3C:BA:D8:76" \
802-11-wireless.mac-address-randomization never \
802-11-wireless.powersave 2 \
802-11-wireless.channel ""
```

GUI: NetworkManager → WiFi → gear icon on VodafoneBAC866 → look for band/BSSID options (if exposed).

Reconnect once (toggle WiFi off/on, or use GUI — avoid hammering connection up repeatedly).

### Step 2 — Fix the driver config - optional

Edit /etc/modprobe.d/iwlwifi.conf to contain only:

options iwlwifi power_save=0
 > Power saving can startle the wifi card. Cheap stability win.

Remove any 11n_disable=1 line. Reboot.
 > This crippled range and throughput on 2.4Ghz, so we disable it.

### Step 3 — Verify

iw dev wlan0 link
nmcli -g 802-11-wireless.band,802-11-wireless.bssid connection show VodafoneBAC866
ping -c 5 192.168.1.1

You want: connected to ...C8:6E, band bg, stable ping to the router.

Or run: ~/wifi-rollback-known-good.sh to re-apply the profile.
