![Topology Diagram](https://github.com/furkangurses/CCNA-Lab-Router-Interface-IP-Configuration-and-Verification/blob/main/4.PNG?raw=true)
# CCNA Lab – Basic Router Interface Configuration & Layer 3 Connectivity

---

## 🎯 Lab Objective

The objective of this lab is to configure IP addresses on Cisco router interfaces, enable those interfaces, and verify end-to-end connectivity between multiple networks using basic Layer 3 routing.

This lab demonstrates how a router:

- Connects different IP networks  
- Acts as a default gateway for hosts  
- Enables inter-network communication  

---

## 🌐 Topology Overview

### Devices

- 1 Router (**R1**)  
- 3 Switches (**SW1, SW2, SW3**)  
- 3 PCs (**PC1, PC2, PC3**)  

Each PC belongs to a different subnet connected to a separate router interface.

---

## ⚙️ Configuration Steps

---

### 1️⃣ Set Router Hostname

```bash
enable
configure terminal
hostname R1
```

---

### 2️⃣ Verify Interfaces (Default State)

```bash
show ip interface brief
```

From configuration mode:

```bash
do show ip interface brief
```

🔎 Observation:

- Interfaces are **administratively down** by default.

---

### 3️⃣ Configure Router Interfaces

---

#### Interface GigabitEthernet 0/0

```bash
interface gigabitethernet 0/0
ip address 15.255.255.254 255.0.0.0
description ## To SW1 ##
no shutdown
```

---

#### Interface GigabitEthernet 0/1

```bash
interface gigabitethernet 0/1
ip address 182.98.255.254 255.255.0.0
description ## To SW2 ##
no shutdown
```

---

#### Interface GigabitEthernet 0/2

```bash
interface gigabitethernet 0/2
ip address 201.191.20.254 255.255.255.0
description ## To SW3 ##
no shutdown
```

---

### 4️⃣ Exit Configuration Mode

```bash
end
```

---

### 5️⃣ Verify Interface Status

```bash
show ip interface brief
```

✔ Interfaces should display:

- **Status:** up  
- **Protocol:** up  

---

### 6️⃣ Save Configuration

```bash
copy running-config startup-config
write memory
```

---

### 7️⃣ Configure PC IP Addresses

On each PC:

- Assign IP address manually  
- Assign correct subnet mask  
- Configure default gateway as the router interface IP  

---

### 8️⃣ Test Connectivity

From each PC:

```bash
ping <destination-ip>
```

Verify successful communication between different networks.

---

## 📝 Commands Used

```bash
enable
configure terminal
hostname R1
show ip interface brief
do show ip interface brief
interface gigabitethernet 0/0
interface gigabitethernet 0/1
interface gigabitethernet 0/2
ip address
description
no shutdown
end
show running-config
copy running-config startup-config
write memory
```

---

## 🧠 Technical Explanation

This lab demonstrates fundamental **Layer 3 routing principles**.

### Key Concepts

- Each router interface must belong to a **different IP subnet**
- Hosts send traffic destined for another network to their **default gateway**
- The router forwards traffic between networks

---

### The `no shutdown` Command

By default, router interfaces are:

```
administratively down
```

The command:

```bash
no shutdown
```

Changes the interface state from **down → up**, activating Layer 1.

---

### Understanding `show ip interface brief`

```bash
show ip interface brief
```

- **Status** → Layer 1 (Physical Layer)  
- **Protocol** → Layer 2 (Data Link Layer)  

Both must show **up/up** for proper operation.

---

### Saving Configuration

Without saving:

- Configuration is stored in **RAM**
- All settings are lost after reboot  

Saving writes configuration to **NVRAM**.

---

## 🌍 Real-World Use Case

This configuration mirrors a small business network:

- Different departments operate on separate subnets  
- A router connects these subnets  
- Inter-department communication depends on correct Layer 3 configuration  

Without:

- Proper IP addressing  
- Correct subnet masks  
- Correct default gateway  

Devices in different networks cannot communicate.

---

## 🛠️ Skills Gained

- Cisco IOS CLI navigation  
- Router interface configuration  
- IP addressing and subnet mask usage  
- Understanding administrative vs operational interface states  
- Default gateway logic  
- Configuration verification  
- Basic network troubleshooting  

---

## ⚡ Possible Improvements

- Add static routes between multiple routers  
- Implement VLAN segmentation  
- Configure DHCP instead of static IP assignment  
- Add SSH for secure remote access  
- Apply basic ACLs for traffic filtering  
- Configure additional security settings  

---

## 🧩 Troubleshooting Notes

If connectivity fails, verify:

```bash
show ip interface brief
```

✔ Interfaces must NOT be administratively down  

Check:

- Correct subnet masks  
- Correct default gateway on PCs  
- IP addresses belong to correct subnet  
- Physical connections in topology  

⚠️ Common Issue:

Forgetting:

```bash
no shutdown
```

This keeps the interface in an **administratively down** state, preventing communication.

---

## 📚 Conclusion

This lab builds core Layer 3 knowledge required for:

- CCNA-level networking  
- IT Support roles  
- Junior Network Engineer positions  

Understanding interface states, IP addressing, and gateway logic is fundamental for real-world network troubleshooting.

---
