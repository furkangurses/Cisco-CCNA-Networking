![Topology Diagram](https://github.com/furkangurses/CCNA-Lab-VLAN-and-Access-Port-Configuration/blob/main/345.PNG?raw=true)

# 🖥️ CCNA Lab – VLAN and Access Port Configuration

---

## 🎯 Lab Objective

The objective of this lab is to practice basic VLAN configuration on a Cisco switch, specifically configuring access ports and enabling inter-VLAN communication through a router. This lab reinforces understanding of VLAN segmentation, IP addressing, and subnetting in a practical network environment.

---

## 🌐 Topology Overview

- **Switch SW1** connecting multiple PCs.

- **Router R1** providing inter-VLAN routing.

**VLANs**:
  
  - VLAN10 – Engineering Department
    
  - VLAN20 – HR Department
    
  - VLAN30 – Sales Department
    
- Each PC is assigned to a specific VLAN and configured with an IP address and default gateway.

---

## ⚙️ Configuration Steps

1. Assign IP addresses, subnet masks, and default gateways to all PCs according to their VLAN.
   
2. Configure R1 router interfaces for each VLAN to enable inter-VLAN routing.
   
3. Connect R1 interfaces to SW1 switchports using straight-through cables.
   
4. Configure SW1 access ports and assign each to the appropriate VLAN.
   
5. Rename VLANs for clarity and verification.
    
6. Test connectivity using `ping` from PCs across VLANs.

---

## 📝 Commands Used

1.Router configuration


enable

configure terminal

interface g0/0

ip address 10.0.0.62 255.255.255.192

no shutdown

interface g0/1

ip address 10.0.0.126 255.255.255.192

no shutdown

interface g0/2

ip address 10.0.0.190 255.255.255.192

no shutdown

exit

2.Switch configuration


enable

configure terminal

interface range g0/1, f3/1, f4/1

switchport mode access

switchport access vlan 10

interface range g1/1, f5/1, f6/1

switchport mode access

switchport access vlan 20

interface range g2/1, f7/1, f8/1

switchport mode access

switchport access vlan 30

3.Rename VLANs

vlan 10

name ENGINEERING

vlan 20

name HR

vlan 30


name SALES

4.Verification

show vlan brief

show ip interface brief

Test connectivity

ping 10.0.0.65

ping 10.0.0.129


---

## 🧠 Technical Explanation

This lab teaches:

- How to **segment a network using VLANs** to improve performance and security.
  
- How **access ports** restrict a switch port to a single VLAN.
  
- The importance of **inter-VLAN routing** via a router, since switches cannot route traffic between VLANs on their own.
  
- How to correctly assign **IP addresses, subnet masks, and default gateways** for proper network communication.
  
- The use of **ping and show commands** for testing and verifying network configurations.

---

## 🌍 Real-World Use Case

- Separating departmental traffic in an enterprise network (Engineering, HR, Sales) using VLANs.
  
- Implementing secure access control and reducing broadcast domains.
  
- Routing inter-department traffic via routers for proper communication.
  
- Troubleshooting connectivity issues in segmented networks.

---

## 🛠️ Skills Gained

- VLAN creation and management
  
- Configuring switch access ports
  
- Router interface configuration for VLAN routing
  
- IP addressing and subnetting practice
  
- Network verification using CLI commands
  
- Troubleshooting basic network connectivity

---

## ⚡ Possible Improvements

- Implement **trunk ports** to allow multiple VLANs over a single link for scalability.
  
- Apply **VLAN security best practices**, such as disabling unused ports and port security.
  
- Integrate **DHCP** for dynamic IP assignment within VLANs.
  
- Document a **network diagram** for visual clarity.

---

## 🧩 Troubleshooting Notes

- Verify VLAN assignment on switch ports: `show vlan brief`.
  
- Confirm router interface IP addresses match PC default gateways.
  
- Ensure all router interfaces are `no shutdown`.
  
- Use `ping` to verify connectivity within and between VLANs.
  
- Check cabling (straight-through for router to switch, switch to PC connections).

