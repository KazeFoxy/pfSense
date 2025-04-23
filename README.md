# pfSense
Firewall pfSense


## General Setup

- Hostname : r-ext
- Domain : gsb.lan
- DNS Servers : 8.8.8.8 et 172.16.0.1
- DNS Server Override : ✅
- Timezone : Europe/Paris
- Timeservers : 2.pfsense.pool.ntp.org
- Language : English



## Interfaces

### WAN
 
 - Enable interface : ✅
 - Description : WAN
 - IPv4 Configuration : DHCP
 - Block private networks and loopback addresses : □

### LAN

 - Enable interface : ✅
 - Description : LAN
 - IPv4 Configuration : Static IPv4 (192.168.200.253 /24)
 - Block private networks and loopback addresses : □

### OTP1 (DMZ)

 - Enable interface : ✅
 - Description : DMZ
 - IPv4 Configuration : Static IPv4 (192.168.100.254 /24)
 - Block private networks and loopback addresses : □

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
