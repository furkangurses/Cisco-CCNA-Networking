![Topology Diagram](https://github.com/furkangurses/CCNA-VLSM-Subnetting-and-Static-Routing-Lab/blob/main/33.PNG?raw=true)

# CCNA VLSM Subnetting and Static Routing Lab

---

## 🎯 Lab Objective

Design and implement a **VLSM-based IP addressing scheme** for the network `192.168.5.0/24`, dividing it into multiple subnets of different sizes, and configure static routing to ensure full inter-LAN connectivity between two routers (R1 and R2).

---

## 🌐 Topology Overview

The topology consists of:

- 2 Routers (R1 and R2)
- 4 LAN segments (LAN1, LAN2, LAN3, LAN4)
- 1 Point-to-Point (P2P) link between routers
- 1 PC per LAN

---

## 📊 Subnet Allocation Strategy (VLSM Order)

Subnetting performed from **largest to smallest**.

| Segment   | Required Hosts | Subnet | Network Address | Broadcast       |
|-----------|---------------|--------|-----------------|-----------------|
| LAN2      | 64            | /25    | 192.168.5.0     | 192.168.5.127   |
| LAN1      | 45            | /26    | 192.168.5.128   | 192.168.5.191   |
| LAN3      | 14            | /28    | 192.168.5.192   | 192.168.5.207   |
| LAN4      | 9             | /28    | 192.168.5.208   | 192.168.5.223   |
| R1-R2 P2P | 2             | /30    | 192.168.5.224   | 192.168.5.227   |

---

### 🧩 Design Principles Applied

- Largest subnet assigned first  
- First usable IP → PC  
- Last usable IP → Router interface  
- /30 used for efficient point-to-point addressing  

---

## ⚙️ Configuration Steps

### 1️⃣ Configure Router Interfaces

- Assign IP addresses according to VLSM plan  
- Use last usable IP for router interfaces  
- Enable interfaces with:

```
no shutdown
```

---

### 2️⃣ Configure PCs

- Assign first usable IP  
- Configure correct default gateway  

---

### 3️⃣ Configure Point-to-Point Link

- Use /30 subnet  
- Assign two usable IP addresses to R1 and R2  

---

### 4️⃣ Configure Static Routes

Each router configures routes for remote LANs using the next-hop IP on the P2P link.

---

### 5️⃣ Verify Connectivity

```
show ip route
show ip interface
ping <remote-ip>
```

---

## 💻 Commands Used

### 🔹 Basic Configuration

```
enable
configure terminal
```

---

### 🔹 Interface Configuration

```
interface g0/1
ip address 192.168.5.126 255.255.255.128
no shutdown
```

```
interface g0/0
ip address 192.168.5.190 255.255.255.192
no shutdown
```

```
interface g0/0/0
ip address 192.168.5.225 255.255.255.252
no shutdown
```

---

### 🔹 Static Routes

```
ip route 192.168.5.128 255.255.255.192 192.168.5.225
ip route 192.168.5.0 255.255.255.128 192.168.5.225
```

---

### 🔹 Verification Commands

```
show ip interface g0/1
show ip route
ping 192.168.5.209
```

---

## 🧠 Technical Explanation

### 🔹 VLSM (Variable Length Subnet Masking)

Instead of splitting a `/24` network evenly, subnets are sized according to actual host requirements.

Benefits:

- Minimizes IP address waste  
- Reflects real enterprise design  
- Improves scalability  

---

### 🔹 Static Routing Logic

Routers only know directly connected networks by default.

Static routes manually define:

- Destination network  
- Subnet mask  
- Next-hop IP address  

This builds the routing table and enables inter-LAN communication.

---

## 🏢 Real-World Use Case

This scenario mirrors a small enterprise network:

- Multiple departments with different host requirements  
- Two routers connecting different segments  
- Manual route configuration between sites  

Common in:

- Small offices  
- Branch networks  
- Lab environments  
- Controlled deployments  

---

## 🛠 Skills Gained

- VLSM-based IP planning  
- CIDR and subnet calculation  
- Layer 3 interface configuration  
- Static routing implementation  
- Routing table interpretation  
- End-to-end connectivity verification  

---

## 🔧 Possible Improvements

- Replace static routing with OSPF  
- Implement VLAN segmentation  
- Configure DHCP services  
- Apply ACLs  
- Introduce redundancy with dynamic routing  

---

## 🐞 Troubleshooting Notes

```
show ip interface
show ip route
```

- Ensure `no shutdown` is applied  
- Confirm default gateway configuration on PCs  
- First ping may fail due to ARP resolution  

---

✅ After proper VLSM allocation and static route configuration, full inter-LAN connectivity between R1 and R2 should be successfully established.
