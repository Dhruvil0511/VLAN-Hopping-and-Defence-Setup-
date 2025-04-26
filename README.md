# 🛡️ VLAN Hopping Prevention & Inter-VLAN Routing | Cisco Packet Tracer

This project demonstrates how to securely set up VLANs in **Cisco Packet Tracer** to prevent VLAN hopping attacks while enabling communication between VLANs using **Router-on-a-Stick**.

---

## 📌 Project Goals

- 🧱 Create VLANs and assign them to ports  
- 🔁 Set up **Inter-VLAN Routing** via Router-on-a-Stick  
- 🔐 Secure the network against VLAN hopping attacks  
- ✅ Validate network connectivity and isolation  
- 🧠 Practice security best practices for switches  

---

## 🖥️ Topology Overview

- **Switch + Router + 3 PCs**
- **VLANs Used:**
  - `VLAN 10` – User_VLAN
  - `VLAN 20` – HR_VLAN
  - `VLAN 30` – Guest_VLAN
  - `VLAN 999` – Native_VLAN (for trunking)

---

## 🧰 Components Used

| Device Type | Quantity |
|-------------|----------|
| Cisco Switch | 1 |
| Cisco Router | 1 |
| PCs | 3 |
| Copper Straight-through cables | as needed |

---

## ⚙️ Configuration Steps

### 🔧 VLAN & Port Configuration (Switch)

```bash
enable
configure terminal

vlan 10
 name User_VLAN
vlan 20
 name HR_VLAN
vlan 30
 name Guest_VLAN
vlan 999
 name Native_VLAN
exit
```

### 🖥️ Assign VLANs to Ports + Security

Repeat with corresponding port numbers:

```bash
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 spanning-tree bpduguard enable
exit
```

### 🌐 Trunk Port to Router

```bash
interface GigabitEthernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30
 switchport nonegotiate
exit
```

### 🔇 Disable Unused Ports

```bash
interface range FastEthernet0/10 - 24
 shutdown
exit
```

### 🔄 VTP Configuration

```bash
vtp mode transparent
```

---

## 🖧 Router Configuration

```bash
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
exit

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
exit

interface GigabitEthernet0/0
 no shutdown
exit
```

---

## 🧪 PC IP Configuration

| PC | VLAN | IP Address | Subnet Mask | Gateway |
|----|------|------------|-------------|---------|
| PC1 | 10 | 192.168.10.2 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 20 | 192.168.20.2 | 255.255.255.0 | 192.168.20.1 |
| PC3 | 30 | 192.168.30.2 | 255.255.255.0 | 192.168.30.1 |

---

## ✅ Verification Commands

| Task | Command |
|------|---------|
| Show VLANs | `show vlan brief` |
| Show Trunks | `show interfaces trunk` |
| Show Port Security | `show port-security` |
| Show ARP Table | `show ip arp` |
| Show Routing | `show ip route` |
| Ping Test | `ping <IP>` |

---

## 🔐 VLAN Hopping Defense Techniques Used

- Changed Native VLAN from default
- Disabled DTP using `switchport nonegotiate`
- Set specific allowed VLANs on trunks
- Enabled **Port Security** with Sticky MAC
- Enabled **BPDU Guard**
- Disabled unused ports
- Set **VTP Mode** to Transparent

---

## 💾 Save Configuration

```bash
Router# copy running-config startup-config
Switch# copy running-config startup-config
```

---

## 📂 Files in This Repo

```
.
├── README.md
├── vlan-hopping-setup.pkt        # Cisco Packet Tracer project file
└── screenshots/                  # (Optional) ping tests, configs, etc.
```

---

## 📸 Optional Screenshots

You can add screenshots of:
- Successful pings between VLANs
- Output of `show vlan brief`
- Output of `show port-security`
- Output of `show ip arp`

---

## ✅ Conclusion

This setup ensures that VLAN communication is secure, controlled, and VLAN hopping is prevented effectively using best practices in a simulated network environment.

---

## 📘 License

This project is open-sourced for educational purposes. Feel free to use, modify, or build upon it.

---

> Created using Cisco Packet Tracer | Securing LANs with VLANs & Inter-VLAN Routing 🚀
