# 🏫 Campus LAN Configuration (Cisco Packet Tracer)

## 📖 Overview
This project simulates a small **campus network** using Cisco Packet Tracer.  
It demonstrates **VLAN segmentation, inter-VLAN routing, trunking, DHCP services, and basic redundancy** in a hierarchical network design (Core–Distribution–Access).

**Goal:** Build a functional and scalable LAN infrastructure suitable for a mid-sized educational campus.

---

## 🧩 Network Design
- **Core Layer:** High-speed routing and inter-VLAN connectivity  
- **Distribution Layer:** VLAN trunk aggregation and redundancy  
- **Access Layer:** End-user connectivity and access security  
- **Services:** DHCP, static routing, and management VLAN

**Key Technologies:**
- VLANs and Trunks  
- Inter-VLAN Routing (Router-on-a-Stick)  
- DHCP Server Configuration  
- Spanning Tree Protocol (optional)  
- Static Routing or OSPF (optional)  
- Network Testing via `ping` and `traceroute`

---

## 🖼️ Topology
![Network Topology](diagrams/topology.png)

*Designed and implemented using Cisco Packet Tracer.*

---

## ⚙️ Device Summary

| Device | Role | VLANs | IP Address | Notes |
|--------|------|--------|-------------|--------|
| Core Router | Layer 3 Gateway | 10, 20, 99 | 10.0.99.1 | Inter-VLAN routing |
| Dist-SW1 | Distribution Switch | 10, 20, 99 | 10.0.99.2 | Trunk to Core |
| Access-SW1 | Access Switch | 10, 20 | - | Hosts connected |
| DHCP Server | Address Assignment | 10, 20 | 10.0.99.10 | Provides IPs |

---

## 🧠 Design Rationale
- **VLAN segmentation** separates network traffic by department (Faculty, Students, Admin).  
- **Router-on-a-stick** enables inter-VLAN communication efficiently.  
- **DHCP** automates address assignment for scalability.  
- **Hierarchical structure** improves manageability and fault isolation.

---

## 🚀 How to Use
1. Open `campus_network.pkt` in Cisco Packet Tracer.
2. Review the **topology** and **device configurations** under `/configs/`.
3. (Optional) Follow the [Setup Guide](docs/setup_guide.md) to rebuild it from scratch.
4. Use the [Verification Tests](docs/verification_tests.md) to confirm connectivity.

---

## ✅ Verification
Example validation commands:
```bash
show ip interface brief
show vlan brief
show interfaces trunk
ping 10.0.10.2
