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
