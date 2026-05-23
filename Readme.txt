Inter-VLAN Routing with Multilayer Switch (VLAN 1-4)

📌 Description

Small project: four departments on separate VLANs routed by a Multilayer Switch without an external router.

· VLAN 1 → HR
· VLAN 2 → IT
· VLAN 3 → SALES
· VLAN 4 → SERVERS

---

🧩 VLAN & IP Plan

Department VLAN ID Subnet Gateway (SVI)
HR 1 192.168.1.0/24 192.168.1.1
IT 2 192.168.10.0/24 192.168.10.1
SALES 3 80.80.0.0/16 80.80.0.1
SERVERS 4 72.80.0.0/16 72.80.0.1

All gateways are Switch Virtual Interfaces (SVI) on the multilayer switch.

---

🌐 Topology

```
[PC-HR] ---- VLAN 1 ----+
[PC-IT] ---- VLAN 2 ----+  Multilayer Switch (Layer 3)
[PC-SALES]-- VLAN 3 ----+
[Server]---- VLAN 4 ----+
```

---

⚙️ Key Configuration (Cisco example)

```cisco
! Enable routing
ip routing

! Define VLANs (VLAN 1 exists by default but we name it)
vlan 1
name HR
vlan 2
name IT
vlan 3
name SALES
vlan 4
name SERVERS

! SVI interfaces (Layer 3 gateways)
interface vlan 1
ip address 192.168.1.1 255.255.255.0
no shutdown

interface vlan 2
ip address 192.168.10.1 255.255.255.0
no shutdown

interface vlan 3
ip address 80.80.0.1 255.255.0.0
no shutdown

interface vlan 4
ip address 72.80.0.1 255.255.0.0
no shutdown

! Assign access ports to VLANs (example for first 5 ports)
interface range fa0/1-5
switchport mode access
switchport access vlan 1
! Repeat for VLAN 2,3,4 on other port ranges
```

---

✔️ Testing

· PC in HR (192.168.1.10) pings PC in IT (192.168.10.10) → success.
· All devices can reach the servers on VLAN 4.

---