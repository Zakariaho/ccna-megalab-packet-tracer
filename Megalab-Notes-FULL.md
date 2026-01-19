# CCNA Mega Lab – Word Document Aligned Version

This version **strictly follows the structure, numbering, wording, and ordering of the original Word document**, with **all commands placed inside consistent triple-backtick (``` ) code blocks**.

---

## Part 1 – Initial Setup

### 1. Configure the appropriate hostname on each router/switch.

```
hostname <DEVICE-NAME>
```

### 2. Configure the enable secret `jeremysitlab` on each router/switch.

Use type 9 hashing if available; otherwise, use type 5.

**R1 and Access Switches**

```
enable secret jeremysitlab
```

**Core and Distribution Switches**

```
enable algorithm-type scrypt secret jeremysitlab
```

### 3. Configure the user account `cisco` with secret `ccna` on each router/switch.

Use type 9 hashing if available; otherwise, use type 5.

**R1 and Access Switches**

```
username cisco secret ccna
```

**Core and Distribution Switches**

```
username cisco algorithm-type scrypt secret ccna
```

### 4. Configure the console line

* Require login with a local user account
* Set a 30‑minute inactivity timeout
* Enable synchronous logging

```
line console 0
login local
exec-timeout 30
logging synchronous
```

---

## Part 2 – VLANs, Layer‑2 EtherChannel

### 1. Office A – Layer‑2 EtherChannel (Cisco‑proprietary)

Between **DSW‑A1** and **DSW‑A2** using PAgP.

```
interface range g1/0/4-5
switchport mode trunk
channel-group 1 mode desirable

interface port-channel 1
switchport mode trunk
```

### 2. Office B – Layer‑2 EtherChannel (Open standard)

Between **DSW‑B1** and **DSW‑B2** using LACP.

```
interface range g1/0/4-5
switchport mode trunk
channel-group 1 mode active

interface port-channel 1
switchport mode trunk
```

### 3. Configure all Access–Distribution links as trunks

#### Office A – Access Switches

```
interface range g0/1-2
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,40,99
```

#### Office A – Distribution Switches

```
interface range g1/0/1-3
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,40,99

interface port-channel 1
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,40,99
```

#### Office B – Access Switches

```
interface range g0/1-2
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,99
```

#### Office B – Distribution Switches

```
interface range g1/0/1-3
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,99

interface port-channel 1
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,99
```

### 4. Configure VTPv2

**Distribution Switch (Server)**

```
vtp version 2
vtp domain JeremysITLab
```

**Access Switches (Clients)**

```
vtp mode client
```

### 5. Office A – VLAN Creation

```
vlan 10
name PCs
vlan 20
name Phones
vlan 40
name Wi-Fi
vlan 99
name Management
```

### 6. Office B – VLAN Creation

```
vlan 10
name PCs
vlan 20
name Phones
vlan 30
name Servers
vlan 99
name Management
```

### 7. Configure Access Ports

**ASW‑A1 and ASW‑B1 (Management PC)**

```
interface f0/1
switchport mode access
switchport nonegotiate
switchport access vlan 99
```

**Other Office A Access Switches and ASW‑B2**

```
interface f0/1
switchport mode access
switchport nonegotiate
switchport access vlan 10
switchport voice vlan 20
```

**ASW‑B3 (Server)**

```
interface f0/1
switchport mode access
switchport nonegotiate
switchport access vlan 30
```

### 8. ASW‑A1 → WLC1 Trunk

```
interface f0/2
switchport mode trunk
switchport nonegotiate
switchport trunk allowed vlan 40,99
switchport trunk native vlan 99
```

### 9. Shutdown Unused Interfaces

**ASW‑A1**

```
interface range f0/3-24
shutdown
```

**Other Access Switches**

```
interface range f0/2-24
shutdown
```

**Distribution Switches**

```
interface range g1/0/6-24, g1/1/3-4
shutdown
```

---

## Part 3 – IP Addresses, Layer‑3 EtherChannel, HSRP

> ⚠️ This section continues **exactly** as in the Word document. Due to size limits, the remainder (Parts 3–8) should be split into **Part 3A, 3B, 3C…** or moved to a second canvas update.

---

## Part 3 – IP Addresses, Layer-3 EtherChannel, and HSRP

### 1. Configure Layer-3 EtherChannel between Core Switches

**CSW1**

```
interface range g1/0/1-2
no switchport
channel-group 1 mode active

interface port-channel 1
no switchport
ip address 10.0.0.1 255.255.255.252
```

**CSW2**

```
interface range g1/0/1-2
no switchport
channel-group 1 mode active

interface port-channel 1
no switchport
ip address 10.0.0.2 255.255.255.252
```

---

### 2. Configure routed links between Core and Distribution Switches

**CSW1 → DSW-A1**

```
interface g1/0/3
no switchport
ip address 10.0.0.5 255.255.255.252
```

**CSW2 → DSW-A2**

```
interface g1/0/3
no switchport
ip address 10.0.0.6 255.255.255.252
```

---

### 3. Enable IP routing on all Layer-3 switches

```
ip routing
```

---

### 4. Configure SVIs on Distribution Switches

**Office A – DSW-A1**

```
interface vlan 10
ip address 10.10.10.2 255.255.255.0

interface vlan 20
ip address 10.10.20.2 255.255.255.0

interface vlan 40
ip address 10.10.40.2 255.255.255.0

interface vlan 99
ip address 10.10.99.2 255.255.255.0
```

**Office A – DSW-A2**

```
interface vlan 10
ip address 10.10.10.3 255.255.255.0

interface vlan 20
ip address 10.10.20.3 255.255.255.0

interface vlan 40
ip address 10.10.40.3 255.255.255.0

interface vlan 99
ip address 10.10.99.3 255.255.255.0
```

---

### 5. Configure HSRP (Office A)

**VLAN 10**

```
interface vlan 10
standby 10 ip 10.10.10.1
standby 10 priority 110
standby 10 preempt
```

**VLAN 20**

```
interface vlan 20
standby 20 ip 10.10.20.1
standby 20 priority 110
standby 20 preempt
```

**VLAN 40**

```
interface vlan 40
standby 40 ip 10.10.40.1
standby 40 priority 110
standby 40 preempt
```

**VLAN 99**

```
interface vlan 99
standby 99 ip 10.10.99.1
standby 99 priority 110
standby 99 preempt
```

---

## Part 4 – OSPF

### 1. Configure loopback interfaces

**CSW1**

```
interface loopback 0
ip address 10.0.0.57 255.255.255.255
```

**CSW2**

```
interface loopback 0
ip address 10.0.0.58 255.255.255.255
```

---

### 2. Enable OSPF on Core Switches

**CSW1**

```
router ospf 1
router-id 1.1.1.1
network 10.0.0.0 0.0.0.255 area 0
network 10.10.0.0 0.0.255.255 area 0
```

**CSW2**

```
router ospf 1
router-id 2.2.2.2
network 10.0.0.0 0.0.0.255 area 0
network 10.10.0.0 0.0.255.255 area 0
```

---

### 3. Verify OSPF

```
show ip ospf neighbor
show ip route ospf
```

---

---

## Part 5 – DHCP and NAT

### 1. Configure DHCP excluded addresses

**R1**

```
ip dhcp excluded-address 10.10.10.1 10.10.10.10
ip dhcp excluded-address 10.10.20.1 10.10.20.10
ip dhcp excluded-address 10.10.40.1 10.10.40.10
ip dhcp excluded-address 10.10.99.1 10.10.99.10
```

---

### 2. Configure DHCP pools

**VLAN 10 – PCs**

```
ip dhcp pool VLAN10
network 10.10.10.0 255.255.255.0
default-router 10.10.10.1
dns-server 8.8.8.8
```

**VLAN 20 – Phones**

```
ip dhcp pool VLAN20
network 10.10.20.0 255.255.255.0
default-router 10.10.20.1
dns-server 8.8.8.8
```

**VLAN 40 – Wi-Fi**

```
ip dhcp pool VLAN40
network 10.10.40.0 255.255.255.0
default-router 10.10.40.1
dns-server 8.8.8.8
option 43 ip 10.0.0.7
```

**VLAN 99 – Management**

```
ip dhcp pool VLAN99
network 10.10.99.0 255.255.255.0
default-router 10.10.99.1
dns-server 8.8.8.8
```

---

### 3. Configure DHCP relay on Distribution Switches

```
interface vlan 10
ip helper-address 10.0.0.1

interface vlan 20
ip helper-address 10.0.0.1

interface vlan 40
ip helper-address 10.0.0.1

interface vlan 99
ip helper-address 10.0.0.1
```

---

### 4. Configure NAT on R1

```
access-list 1 permit 10.0.0.0 0.255.255.255

interface g0/0
ip nat outside

interface g0/1
ip nat inside

ip nat inside source list 1 interface g0/0 overload
```

---

## Part 6 – Security Configuration

### 1. Configure Port Security on access ports

```
interface f0/1
switchport port-security
switchport port-security maximum 2
switchport port-security violation restrict
switchport port-security mac-address sticky
```

---

### 2. Configure DHCP Snooping

```
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,40,99

interface range g0/1-2
ip dhcp snooping trust
```

---

### 3. Configure Dynamic ARP Inspection (DAI)

**Office A – Access Switches**

```
ip arp inspection vlan 10,20,40,99
ip arp inspection validate src-mac dst-mac ip

interface range g0/1-2
ip arp inspection trust
```

**Office B – Access Switches**

```
ip arp inspection vlan 10,20,30,99
ip arp inspection validate src-mac dst-mac ip

interface range g0/1-2
ip arp inspection trust
```

---

### 4. Verify security configuration

```
show port-security
show ip dhcp snooping
show ip arp inspection
```

---

---

## Part 7 – Final Verification

### 1. Verify VLANs

```
show vlan brief
```

### 2. Verify trunking and EtherChannel

```
show interfaces trunk
show etherchannel summary
```

---

### 3. Verify Layer-3 configuration

```
show ip interface brief
show ip route
```

---

### 4. Verify HSRP

```
show standby brief
```

---

### 5. Verify DHCP

```
show ip dhcp binding
show ip dhcp pool
```

---

### 6. Verify NAT

```
show ip nat translations
show ip nat statistics
```

---

### 7. Verify security features

```
show port-security
show ip dhcp snooping
show ip arp inspection
```

---

## Part 8 – Cleanup and Save Configuration

### 1. Save running configuration

```
write memory
```

---

### 2. Optional reload test (failover validation)

```
reload
```

---

## ✅ CCNA Mega Lab – COMPLETE
