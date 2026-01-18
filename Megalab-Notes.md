# CCNA Mega Lab – Configuration Notes

These notes document the summarised configuration of my **CCNA Mega Lab** based on **Jeremy’s IT Lab**.  
They are organized by section to match the lab structure and to make verification easier.

---

## Part 1 – Initial Device Setup

### Common Configuration (All Routers & Switches)
- Hostname configured
- Enable secret: `jeremysitlab`
- Local user:
  ```
  username cisco secret ccna
  ```
- Console:
  ```
  line console 0
   login local
   exec-timeout 30
   logging synchronous
  ```

---

## Part 2 – VLANs & Layer-2 EtherChannel

### VLANs
#### Office A
| VLAN | Name |
|----|------|
| 10 | PCs |
| 20 | Phones |
| 40 | Wi‑Fi |
| 99 | Management |

#### Office B
| VLAN | Name |
|----|------|
| 10 | PCs |
| 20 | Phones |
| 30 | Servers |
| 99 | Management |

### Trunking Rules
- Native VLAN: **1000**
- DTP disabled on all trunks
- VLANs explicitly allowed per office

### EtherChannel
- **Office A**: PAgP (desirable)
- **Office B**: LACP (active)

---

## Part 3 – IP Addressing, Layer-3 EtherChannel, HSRP

### R1 Interfaces
| Interface | IP |
|---------|----|
| G0/0 | 10.0.0.33/30 |
| G0/1 | 10.0.0.37/30 |
| Loopback0 | 10.0.0.76/32 |

### HSRP (Summary)
| Office | VLAN | VIP |
|------|------|-----|
| A | 99 | 10.0.0.1 |
| A | 10 | 10.1.0.1 |
| A | 20 | 10.2.0.1 |
| A | 40 | 10.6.0.1 |
| B | 99 | 10.0.0.17 |
| B | 10 | 10.3.0.1 |
| B | 20 | 10.4.0.1 |
| B | 30 | 10.5.0.1 |

---

## Part 4 – Spanning Tree

- Rapid PVST+ enabled
- Root bridge aligned with HSRP active router
- PortFast + BPDU Guard on access ports

---

## Part 5 – Routing

### OSPF
- Process ID: 1
- Area: 0
- Router IDs match loopback IPs
- Point‑to‑point links where applicable
- R1 advertises default route

---

## Part 6 – Network Services

### DHCP
- R1 acts as DHCP server
- First 10 usable addresses excluded
- Pools:
  - A‑Mgmt, A‑PC, A‑Phone
  - B‑Mgmt, B‑PC, B‑Phone
  - Wi‑Fi

### DNS / NTP / SNMP / Syslog
- DNS server: **SRV1 (10.5.0.4)**
- NTP server: **R1**
- SNMP (RO): `SNMPSTRING`
- Syslog sent to SRV1

### NAT
- Static NAT for SRV1
- Dynamic PAT using pool `203.0.113.200–207`

---

## Part 7 – Security

- Extended ACL: Office A → Office B control
- Port Security (sticky MACs)
- DHCP Snooping
- Dynamic ARP Inspection

---

## Part 8 – IPv6

- IPv6 routing enabled
- EUI‑64 addressing
- Dual default routes (floating backup)

---

## Part 9 – Wireless

- WLC1 management: `10.0.0.7`
- WLAN:
  - SSID: **Wi‑Fi**
  - VLAN: 40
  - Security: WPA2‑PSK (AES)

> ⚠️ Wireless client DHCP limitations exist in Packet Tracer.

---

## Notes
- Tested on **Cisco Packet Tracer 8.2+**
- For educational use only
