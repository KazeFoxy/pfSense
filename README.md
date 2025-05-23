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
 - Block bogon networks : □

### LAN

 - Enable interface : ✅
 - Description : LAN
 - IPv4 Configuration : Static IPv4 (192.168.200.253 /24)
 - Block private networks and loopback addresses : □
 - Block bogon networks : □
   
### OTP1 (DMZ)

 - Enable interface : ✅
 - Description : DMZ
 - IPv4 Configuration : Static IPv4 (192.168.100.254 /24)
 - Block private networks and loopback addresses : □
 - Block bogon networks : □


## pfSense NAT

### Port Forward

 - WAN
 - IPv4
 - TCP
 - Destination : This Firewall (self)
 - Destination port range : From HTTPS - To HTTPS
 - Redirecte target IP : Address or Alias - 192.168.100.250
 - Redirecte target port : HTTPS
 - Description : Rediriger HTTPS vers DMZ depuis le WAN



## pfSense Rules

### WAN

 - Action : Pass
 - Interface : WAN
 - Address Family : IPv4
 - protocol : TCP
 - Source : ANY
 - Destination : Address or Alias : 192.168.100.250
 - Destination Port Range : From HTTPS To HTTPS
 - Description : NAT Rediriger HTTPS vers DMZ depuis le WAN

> :bulb: Règle créée automatiquement !

### LAN

1er Règle :
 - Action : Pass
 - Interface : LAN
 - Address Family : IPv4
 - protocol : TCP
 - Source : Network 172.16.64.0 /24
 - Destination : Address or Alias : 192.168.100.250
 - Destination Port Range : From HTTPS To HTTPS
 - Description : HTTPS Réseau User vers Guacamole

2ème Règle :
 - Action : Pass
 - Disabled : ✅
 - Interface : LAN
 - Address Family : IPv4
 - protocol : ICMP
 - Source : Network 172.16.64.0 /24
 - Destination : Address or Alias : 192.168.100.250
 - Description : Ping Réseau User vers Guacamole
> :bulb: Règle à activer seulement pour des testes de ping

3ème Règle :
 - Action : Pass
 - Disabled : ✅
 - Interface : LAN
 - Address Family : IPv4
 - protocol : ICMP
 - Source : ANY
 - Destination : This Firewall (self)
 - Description : Ping vers FW LAN
> :bulb: Règle à activer seulement pour des testes de ping

### DMZ

1er Règle :
 - Action : Pass
 - Disabled : ✅
 - Interface : DMZ
 - Address Family : IPv4
 - protocol : TCP
 - Source : Address or Alias : 192.168.100.250
 - Destination : Network 172.16.64.0 /24
 - Destination Port Range : From HTTPS To HTTPS
 - Description : Gucamole vers Réseau User

2ème Règle :
 - Action : Pass
 - Disabled : ✅
 - Interface : DMZ
 - Address Family : IPv4
 - protocol : TCP
 - Source : Address or Alias : 192.168.100.250
 - Destination : Network 172.16.0.0 /24
 - Destination Port Range : From MS RDP To MS RDP
 - Description : Gucamole RDP vers Réseau Infra

3ème Règle : 
 - Action : Pass
 - Disabled : ✅
 - Interface : DMZ
 - Address Family : IPv4
 - protocol : TCP
 - Source : Address or Alias : 192.168.100.250
 - Destination : Network 172.16.0.0 /24
 - Destination Port Range : From SSH To SSH
 - Description : Gucamole SSH vers Réseau Infra

4ème Règle : 
 - Action : Pass
 - Disabled : ✅
 - Interface : DMZ
 - Address Family : IPv4
 - protocol : ICMP
 - Source : DMZ Subnets
 - Destination : This Firewall (self)
 - Description : DMZ ICMP vers FW (r-ext)



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
 - Description : Route vers réseau Infra

2ème route : 

 - Destination Network : 172.16.64.0 /24
 - Gateway : RextToRint - 192.168.200.254
 - Description : Route vers réseau User
