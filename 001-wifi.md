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

 > My problem was that there were hiccups during connect/disconnect. So every time my router band steered me between the bands,
 > there would be interruption to service. It was fixed in the *Network Manager*, by forcing my laptop to stay on one of the two
 > AP's.


 ddfdfd
