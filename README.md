# pfSense
Firewall pfSense


## pfSense Rules

### WAN


### LAN


### DMZ


## pfSense NAT


## pfSense Routing

### Gateways

 - Interface : LAN
 - Name : RextToRint
 - Gateway : 192.168.200.254 (r-int)
 - Description : R-ext to R-int

### Static Routes

1er route :

 - Destination Network : 172.16.0.0 /24
 - Gateway : RextToRint - 192.168.200.254
 - Description : Route vers réseau infra

2ème route : 

 - Destination Network : 172.16.64.0 /24
 - Gateway : RextToRint - 192.168.200.254
 - Description : Route vers réseau user
