# Overview
In this file I will walk through every action I took while configuring the LAN for an in-depth look into how the devices were configured from beginning to end. To see final device configurations, please see the `/configs/` file

</br>

## Initial setup
- Configured devices with the appropriate hostnames (R1, CSW1, DSW-A1, etc.)
- Configured the enable secret 'jeremysitlab' on all routers and switches
- Configured user accounts (user: cisco, pass: ccna) on all routers and switches
- Configured devices to require login through the local user account when accessing through the console port

</br>

## VLANs, VTP, and Layer-2 EtherChannel
- Configured etherchannel in office A using PaGP between DSW-A1 and DSW-A2
- Configured etherchannel in office B using LACP between DSW-B1 and DSW-B2
- Configured all links between Access and Distribution switches, including the etherchannels, as trunk links. Also, disabled DTP, set the native vlan to an unused vlan, and only allowed the necessary vlans for each office space
- Configured DSW-A1 and DSW-A2 as VTPv2 servers under the domain ciscodomain
- Configured Access switches as VTP clients
- Configured VLANs:
  - VLAN10 - PCs
  - VLAN20 - Phones
  - VLAN30 - Servers (Office B only)
  - VLAN40 - WiFi (Office A only)
  - VLAN99 - Management
- Configured access ports on Access switches, ensuring they follow the vlan topology
- Configured trunk port from ASW-A1 to the WLC as a trunk port with the management VLAN untagged
- Administratively disabled all unused Access and Distribution Switch ports

</br>

## IPv4 Addresses, Layer-3 Etherchannel, HSRP, and Rapid-PVST+
- Assigned addresses to R1's external facing ports through dhcp and internal facing ports statically
- Enabled IP routing on Core and Distribution switches
- Created a layer 3 etherchannel connection between CSW1 and CSW2 using PaGP and assigned IP addresses
- Assigned static addresses to CSW1 and CSW2 on the interfaces connecting them to the Distribution switches in Offices A and B as well as to the interfaces connecting to R1
- Assigned static addresses to the loopback interfaces for R1, Core switches, and Distribution switches
- Disabled all unused interfaces on Core switches
- Assigned static addresses on Distribution switch interfaces leading to the Core switches and on the loopback interfaces
- Statically configured SRV1's (server 1) address, default gateway, and subnet mask
- Statically configured management addresses for all Access switches on the management VLAN interface
- Configured HSRP on all Distribution switches to provide redundancy and load-balanced traffic from each subnet between them, VLANs 10 and 99 use DSW-A1/DSW-B1 as the active device in their respective office, VLANs 20 and 30/40 use DSW-A2/DSW-B2 as the active device in their respective office
- Configured Rapid-PVST+ to align with the load-balancing of VLANs configured in HSRP
- Configured PortFast and BPDU Guard on all access ports in the Access layer

</br>

## Single area OSPF and Static Routes
- Configured OSPF on R1 and all Core and Distribution switches with a process ID of 1 in area 0
  - OSPF RID's were configured to match loopback interfaces
  - Loopbacks and SVIs were configured as passive interfaces
  - All connections between OSPF enabled devices were configured as point-to-point to prevent DR/BDR elections
  - R1 configured as the ASBR and was given static default routes to its internet connections

</br>

## Services - DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT
- DHCP
  - Created dhcp pools A-Mgmt, B-Mgmt, A-PC, B-PC, A-Phone, B-Phone, Wi-Fi.
  - Assigned appropriate networks and default gateways according to VLANs
  - Assigned all pools with the same domain name and DNS servers
  - Assigned option 43 to the A/B-Mgmt pools with the WLC address
  - Configured Distribution Switches to relay DHCP broadcasts from each VLANs SVI to R1's loopback address
- DNS
  - Configured SRV1's DNS service with four records: youtube.com, google.com, jeremysitlab.com, and www.jeremysitlab.com (as CNAME record)
  - Configured all routers and switches to use domain jeremysitlab.com and SRV1 as the DNS server
- NTP
  - Configured R1 as a Stratum 5 NTP server
  - Configured all Core, Dist, and Access Switches to use R1's loopback as their ntp server
- SNMP
  - Configured all Core, Dist, and Access Switches with Read-only community string SNMPSTRING
- Syslog
  - Configured logging of messages of all severity levels to SRV1 from all Routers and Switches
  - Enabled logging to the buffer up to 8192 bytes
- FTP
  - Configured FTP Username Cisco and Password Cisco on R1
  - Used FTP on R1 to download a new IOS version from SRV1
  - Rebooted with new version and removed old version from flash:
- SSH All routers and switches
  - Generated 4096 bit RSA keys
  - Allow only SSHv2
  - Created standard ACL 1 to permit connections only from Office A PCs and applied to all VTY lines
  - Restricted VTY access to only SSH input
  - Configured to require local login on VTY
  - Configured synchronous logging on VTY
- NAT
  - Configured static NAT on R1 to enable internet hosts to access SRV1 via IP Add 203.0.113.113
  - Configured ACL 2 to permit access from the PC and Phone Subnets in office A and B as well as the Wi-Fi subnet
  - Created POOL1 to provide 203.0.113.200 - 203.0.113.207 as public addresses to be NAT'd
  - Tied ACL 2 to POOL1 and enabled PAT overload

</br>

## Security: ACLs and Layer-2 Security
- Configured extended ACL on CSW2 leading into Office B
  - Allow ICMP from Office A PCs to Office B PCs
  - Blocked all other traffic from Office A to Office B
  - Allowed all other traffic
- Configured Port Security on each Access Switch's F0/1 port
  - Allowed the minimum MAC addresses necessary
  - Configured in Restrict mode
  - Configured sticky MAC-addresses
- Configured DHCP Snooping on all Access switches
  - Enabled on all active VLANs in the LAN
  - Trusted ports connecting to other switches and to SRV1
  - Disabled insertion of option 82
  - Set rate limit of 15 pps on untrusted ports
  - Set a higher limit of 100 pps on ASW-A1s connection to the WLC
- Configured Dynamic ARP Inspection on all Access switches
  - Enabled on all active VLANs in the LAN
  - Enabled all optional inspections (IP, src-mac, dst-mac)
  - Trusted connections between switches

</br>

## IPv6
- Enabled IPv6 Routing and configured addresses on R1, CSW1, and CSW2
- Configured default static routes on R1 leading to the internet
- Enabled IPv6 on Port Channel 1 on CSW1 and CSW2 without adding an address
