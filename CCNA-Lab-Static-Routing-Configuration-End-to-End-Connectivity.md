![Topology Diagram](https://github.com/furkangurses/CCNA-Lab-Static-Routing-Configuration-End-to-End-Connectivity/blob/main/4423.PNG?raw=true)

# CCNA Lab – Static Routing Configuration & End-to-End Connectivity

---

## 🎯 Lab Objective

Configure **static routing** on Cisco routers to enable full bidirectional connectivity between two remote LANs.

Goal:

- Allow **PC1 (192.168.1.0/24)**  
- To communicate with **PC2 (192.168.3.0/24)**  
- Across three routers (**R1 – R2 – R3**)  

---

## 🌐 Topology Overview

### Devices Used

- 3 × Cisco Routers (**R1, R2, R3**)  
- 2 × PCs (**PC1, PC2**)  
- 2 × Layer 2 Switches  

---

### 🧮 IP Addressing Plan

| Device | Interface | IP Address      | Subnet |
|--------|----------|----------------|--------|
| PC1    | Fa0      | 192.168.1.1    | /24    |
| R1     | G0/1     | 192.168.1.254  | /24    |
| R1     | G0/0     | 192.168.12.1   | /24    |
| R2     | G0/0     | 192.168.12.2   | /24    |
| R2     | G0/1     | 192.168.13.2   | /24    |
| R3     | G0/0     | 192.168.13.3   | /24    |
| R3     | G0/1     | 192.168.3.254  | /24    |
| PC2    | Fa0      | 192.168.3.1    | /24    |

---

### Default Gateways

- **PC1 → 192.168.1.254**
- **PC2 → 192.168.3.254**

---

## ⚙️ Configuration Steps

---

### 1️⃣ Basic Device Configuration

- Set hostnames (R1, R2, R3)  
- Assign IP addresses to router interfaces  
- Enable interfaces using `no shutdown`  
- Configure PC IP addresses and default gateways  

---

### 2️⃣ Static Route Configuration

Required static routes:

- R1 → Route to **192.168.3.0/24**
- R2 → Route to **192.168.1.0/24**
- R2 → Route to **192.168.3.0/24**
- R3 → Route to **192.168.1.0/24**

✔ Total: **4 static routes**

---

## 💻 Router Configurations

---

### 🔹 R1 Configuration

```bash
enable
configure terminal
hostname R1

interface g0/1
ip address 192.168.1.254 255.255.255.0
description ## to SW1 ##
no shutdown

interface g0/0
ip address 192.168.12.1 255.255.255.0
description ## to R2 ##
no shutdown

ip route 192.168.3.0 255.255.255.0 192.168.12.2

show ip interface brief
show ip route
```

---

### 🔹 R2 Configuration

```bash
enable
configure terminal
hostname R2

interface g0/0
ip address 192.168.12.2 255.255.255.0
description ## to R1 ##
no shutdown

interface g0/1
ip address 192.168.13.2 255.255.255.0
description ## to R3 ##
no shutdown

ip route 192.168.1.0 255.255.255.0 g0/0
ip route 192.168.3.0 255.255.255.0 192.168.13.3

show ip interface brief
show ip route
```

---

### 🔹 R3 Configuration

```bash
enable
configure terminal
hostname R3

interface g0/0
ip address 192.168.13.3 255.255.255.0
description ## to R2 ##
no shutdown

interface g0/1
ip address 192.168.3.254 255.255.255.0
description ## to SW2 ##
no shutdown

ip route 192.168.1.0 255.255.255.0 192.168.13.2

show ip interface brief
show ip route
```

---

### 🔹 Connectivity Test

```bash
ping 192.168.3.1
```

---

## 🧠 Technical Explanation

This lab demonstrates how **static routing** works in a multi-router topology.

### Key Concepts

- Connected vs Local vs Static routes  
- Next-hop IP vs Exit interface configuration  
- Bidirectional routing requirement  
- Basic ARP behavior during first ping  

Static routing requires **manual route definition**, meaning administrators must explicitly define how traffic reaches remote networks.

---

## 🌍 Real-World Use Case

Static routing is commonly used in:

- Small enterprise networks  
- Stub networks  
- Branch offices with single exit paths  
- Lab environments  
- Floating static route backups  

Advantages:

- Predictable routing behavior  
- Minimal CPU overhead  
- No routing protocol traffic  

---

## 🛠️ Skills Gained

- Cisco IOS CLI proficiency  
- Interface configuration  
- IP addressing and subnetting  
- Static route configuration  
- Routing table verification  
- Understanding packet flow across multiple routers  

---

## 🔧 Possible Improvements

- Replace static routes with OSPF  
- Configure floating static routes  
- Implement default routes  
- Secure device access (SSH)  
- Disable unused interfaces  
- Save configurations (`write memory`)  

---

# 🛠️ Static Routing Troubleshooting Lab

---

## 🎯 Lab Objective

Troubleshoot and fix routing issues in a previously configured static routing topology.

Goal:

Identify and correct **one misconfiguration per router (R1, R2, R3)** to restore connectivity between:

- PC1 → 192.168.1.0/24  
- PC2 → 192.168.3.0/24  

---

## 🔍 Troubleshooting Methodology

1. Verify problem (ping test)  
2. Check PC configuration  
3. Verify interface status  
4. Check routing tables  
5. Identify incorrect static routes or IP addressing  
6. Correct misconfigurations  
7. Re-test connectivity  

---

## ⚠️ Configuration Issues Identified

---

### 🔴 Issue 1 – R1 Incorrect Next-Hop IP

**Incorrect:**

```bash
ip route 192.168.3.0 255.255.255.0 192.168.12.3
```

**Correct:**

```bash
ip route 192.168.3.0 255.255.255.0 192.168.12.2
```

---

### 🔴 Issue 2 – R2 Incorrect Exit Interface

**Incorrect:**

```bash
ip route 192.168.3.0 255.255.255.0 g0/0
```

**Correct:**

```bash
ip route 192.168.3.0 255.255.255.0 g0/1
```

Problem caused unintended load balancing.

---

### 🔴 Issue 3 – R3 Incorrect Interface IP

**Incorrect:**

```
192.168.23.3
```

**Correct:**

```
192.168.13.3
```

Layer 3 mismatch broke connectivity between R2 and R3.

---

## 💻 Troubleshooting Commands

---

### 🔹 PC Verification

```bash
ipconfig
ipconfig /all
ping 192.168.1.254
ping 192.168.3.1
```

---

### 🔹 R1 Fix

```bash
enable
show ip interface brief
show ip route
show running-config | include ip route

configure terminal
no ip route 192.168.3.0 255.255.255.0 192.168.12.3
ip route 192.168.3.0 255.255.255.0 192.168.12.2
show ip route
```

---

### 🔹 R2 Fix

```bash
enable
show ip interface brief
show ip route
show running-config | include ip route

configure terminal
no ip route 192.168.3.0 255.255.255.0 g0/0
ip route 192.168.3.0 255.255.255.0 g0/1
show ip route
```

---

### 🔹 R3 Fix

```bash
enable
show ip interface brief
show running-config

configure terminal
interface g0/0
ip address 192.168.13.3 255.255.255.0

show ip route
```

---

## 🧠 Troubleshooting Concepts Reinforced

- Structured Layer 3 troubleshooting  
- Routing table analysis  
- IOS filtering (`| include`)  
- ECMP behavior understanding  
- Next-hop validation  
- Layer 3 addressing consistency  

Even a **single digit mistake** can break end-to-end communication.

---

## 🐞 Troubleshooting Notes

- Always verify interface status first  
- Incorrect next-hop = packet blackhole  
- Wrong exit interface may cause load balancing issues  
- IP mismatches prevent neighbor connectivity  
- First ping may fail due to ARP resolution  

---

## ✅ Final Result

After correcting all three misconfigurations:

✔ PC1 successfully pings PC2  
✔ Full bidirectional connectivity restored  
✔ Routing tables contain correct static entries  

---

## 📚 Conclusion

This lab strengthens:

- Static route configuration  
- Routing logic understanding  
- Structured troubleshooting methodology  
- Real-world problem isolation skills  

Essential for CCNA-level routing and entry-level network engineering roles.

---
