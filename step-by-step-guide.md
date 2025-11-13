# Overview
In this file I will walk through every action I took while configuring the LAN for an in-depth look into how the devices were configured. To see final device configurations, please see the `/configs/` file

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

