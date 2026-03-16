![Topology Diagram](https://github.com/furkangurses/LAN-Setup-Router-and-Switch-Interface-Configuration-CCNA-/blob/main/45.PNG?raw=true)

# CCNA Lab – Single LAN Router & Switch Configuration

---

## 🎯 Lab Objective

Configure and verify interfaces on:

- 1 Router (**R1**)  
- 2 Switches (**SW1, SW2**)  

Within a single LAN:

```
Network: 172.16.0.0/16
Gateway: 172.16.255.254 (R1 G0/0)
```

This lab focuses on:

- Hostname configuration  
- IP addressing  
- Speed and duplex settings  
- Interface documentation  
- Disabling unused ports  
- Saving configurations  

---

## 🌐 Topology Overview

```
                172.16.0.0/16

                 R1
              G0/0 | 172.16.255.254
                    |
                  SW1
         -----------|------------
        |                        |
      SW2                     PC1, PC2
                                |
                             PC3, PC4
```

### Devices

- **R1** – Default gateway for the LAN  
- **SW1** – Distribution switch  
- **SW2** – Access switch  
- **PC1–PC4** – End devices  

---

## ⚙️ Configuration Steps

---

### 1️⃣ Configure Hostnames

```bash
hostname R1
hostname SW1
hostname SW2
```

---

### 2️⃣ Configure IP Addressing

#### Router – Interface G0/0

```bash
interface g0/0
ip address 172.16.255.254 255.255.0.0
no shutdown
```

---

### PC IP Configuration

| PC  | IP Address   | Subnet Mask   | Default Gateway   |
|-----|--------------|--------------|-------------------|
| PC1 | 172.16.0.1   | 255.255.0.0  | 172.16.255.254    |
| PC2 | 172.16.0.2   | 255.255.0.0  | 172.16.255.254    |
| PC3 | 172.16.0.3   | 255.255.0.0  | 172.16.255.254    |
| PC4 | 172.16.0.4   | 255.255.0.0  | 172.16.255.254    |

---

### 3️⃣ Configure Speed and Duplex

Configured manually on Gigabit interfaces:

```bash
speed 1000
duplex full
```

---

### 4️⃣ Add Interface Descriptions

Used to document:

- Inter-device links  
- Access ports  
- Unused ports  

Example:

```bash
description ## to SW1 ##
description ## host ports ##
description ## not in use ##
```

---

### 5️⃣ Disable Unused Interfaces

All unused FastEthernet interfaces were administratively shut down:

```bash
shutdown
```

This improves:

- Security  
- Network hygiene  
- Operational clarity  

---

### 6️⃣ Save Configurations

```bash
copy running-config startup-config
write memory
write
```

---

## 📝 Commands Used

---

### 🖥 Router (R1)

```bash
enable
configure terminal
hostname R1
interface g0/0
ip address 172.16.255.254 255.255.0.0
speed 1000
duplex full
description ## to SW1 ##
no shutdown
interface range g0/1 - 2
description ## not in use ##
do show ip interface brief
show running-config
copy running-config startup-config
show startup-config
```

---

### 🖥 Switch SW1

```bash
enable
configure terminal
hostname SW1
interface g0/1
speed 1000
duplex full
description ## to R1 ##
interface g0/2
speed 1000
duplex full
description ## to SW2 ##
interface range f0/1 - 2
description ## host ports ##
interface range f0/3 - 24
description ## not in use ##
shutdown
show interface status
write memory
show startup-config
```

---

### 🖥 Switch SW2

```bash
enable
configure terminal
hostname SW2
interface g0/1
speed 1000
duplex full
description ## to SW1 ##
interface range f0/1 - 2
description ## host ports ##
interface range g0/2, f0/3 - 24
description ## not in use ##
shutdown
show interface status
write
show startup-config
```

---

## 🧠 Technical Explanation

This lab reinforces fundamental enterprise networking concepts:

- Router interfaces are **administratively down by default** and must be enabled.
- Switch interfaces are **enabled by default**, but should be secured.
- Proper Layer 3 addressing enables LAN communication.
- Speed and duplex must match on both ends to prevent performance issues.
- Configurations must be saved to persist after reboot.

---

## 🌍 Real-World Use Case

This configuration reflects a small office LAN deployment scenario.

Applicable in:

- New branch office setups  
- Lab environments  
- Entry-level network engineering roles  
- IT support infrastructure initialization tasks  

Proper interface documentation and unused port shutdown are standard enterprise best practices.

---

## 🛠️ Skills Gained

- Cisco IOS CLI navigation  
- Interface configuration and verification  
- IP addressing implementation  
- Manual speed and duplex configuration  
- Interface documentation best practices  
- Bulk configuration using `interface range`  
- Configuration persistence management  

---

## 🔮 Possible Improvements

- Implement VLAN segmentation  
- Configure inter-VLAN routing  
- Enable SSH for secure remote management  
- Apply port security on access ports  
- Implement basic ACLs  
- Configure NTP and logging  

---

## 🧩 Troubleshooting Notes

Use:

```bash
show ip interface brief
```

To verify router interface status.

Use:

```bash
show interface status
```

On switches to verify port states.

Check:

- Speed and duplex match on both ends  
- Router interfaces are not missing `no shutdown`  
- Correct IP addressing  
- Configuration is saved  

⚠ Packet Tracer may display auto-negotiation indicators inaccurately in some scenarios.

---

## 📚 Conclusion

This lab builds strong foundational skills in:

- Router and switch initialization  
- LAN deployment best practices  
- Interface documentation  
- Basic enterprise configuration standards  

Essential knowledge for CCNA preparation, IT Support, and Junior Network roles.

---
