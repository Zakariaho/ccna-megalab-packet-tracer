# CCNA Mega Lab – Packet Tracer  
**Based on Jeremy’s IT Lab**

This repository contains a full **CCNA Mega Lab** implemented in **Cisco Packet Tracer**.  
It is designed for learning, practice, and verification of CCNA concepts.

---

## 📂 Repository Contents

- **ccna-megalab.pka**  
  The main Packet Tracer lab file containing all devices and configurations.

- **Megalab-Notes-FULL.md**  
  Complete lab instructions and configuration commands.

- **Megalab-Notes.md**  
  A summarized, high-level version of the lab for quick reference.

- **addressing-table.pdf**  
  IP addressing, VLANs, gateways, and HSRP VIPs for reference.

- **topology.png**  
  Diagram of the full network topology for visual reference.

---

## 🧪 Topics Covered

- VLANs & Trunking  
- Layer 2 & Layer 3 EtherChannel  
- Rapid PVST+  
- Inter-VLAN Routing  
- HSRP (v2)  
- OSPF (single-area)  
- Static & Dynamic Routing  
- DHCP, DNS, NTP, SNMP, Syslog  
- NAT & PAT  
- ACLs & Layer 2 Security  
- IPv6 (dual-stack)  
- Wireless (WLC & LWAP – limited in Packet Tracer)

---

## ▶️ How to Use

1. Download the `.pka` lab file (`ccna-megalab.pka`)  
2. Open it in **Cisco Packet Tracer (v8.2 or newer recommended)**  
3. Use `Megalab-Notes-FULL.md` to follow each step of the lab or verify configurations  
4. Reference `addressing-table.pdf` and `topology.png` for IPs, VLANs, HSRP VIPs, and network layout

---

## 🔐 Traffic & Reachability Summary

| Source            | Destination           | Allowed | Control Mechanism |
|-------------------|----------------------|---------|-------------------|
| Same VLAN hosts   | Same VLAN hosts      | Yes     | Layer 2 switching |
| Office A PCs (10.1.0.0/24) | Office B PCs (10.3.0.0/24) | ICMP only | Extended ACL `OfficeA_to_OfficeB` |
| Office A PCs      | Office B PCs         | No      | Explicit ACL deny |
| User VLANs        | Internet / WAN       | Yes     | PAT using ACL 2   |
| Wi-Fi VLAN        | Internet / WAN       | Yes     | PAT using ACL 2   |
| Unauthorized VLANs| Restricted networks  | No      | ACL enforcement  |

---
### 🔑 Management & Authentication Controls
Username: cisco
Password: ccna
Priviliged access: Jeremysitlab

---

## ⚠️ Completion Status & Limitations

This Mega Lab is **not 100% complete** due to **Cisco Packet Tracer limitations**, not configuration errors.

### Known Limitations
- **Wireless DHCP functionality** does not fully operate as expected in Packet Tracer.
- Some **WLC and LWAP behaviors** differ from real Cisco hardware.
- Certain advanced features may appear configured correctly but do not function fully in simulation.
- Minor **command spelling variations** may exist in documentation; these do not affect functionality and are not counted toward the completion percentage.

### What *Is* Complete
- All routing, switching, and security configurations were implemented correctly.
- ACL behavior, inter-VLAN routing, NAT/PAT, and management access function as intended.
- Any minor documentation typos do not impact network operation.
- Configuration aligns with **CCNA-level expectations** and Jeremy’s IT Lab design.

### Summary
Any incomplete behavior is caused by **simulation constraints or documentation formatting**, not missing or incorrect configurations.  
The lab remains valid for **learning, verification, and CCNA exam preparation**.

---

## 🛠️ Tools Used

- Cisco Packet Tracer  
- GitHub for version control and documentation  

---

## 📅 Tested Environment

- Cisco Packet Tracer 8.2+
