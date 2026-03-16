![Topology Diagram](https://github.com/furkangurses/Inter-VLAN-Routing-with-802.1Q-Trunking-Router-on-a-Stick-Lab-/blob/main/3456.PNG?raw=true)

# 🖥️ CCNA Lab – VLAN and Trunk Configuration

---

## 🎯 Lab Objective

The objective of this lab is to practice VLAN configuration on Cisco switches, specifically configuring access ports, creating trunk links, and enabling inter-VLAN routing through a router. This lab reinforces understanding of VLAN segmentation, trunking, IP addressing, and inter-VLAN communication in a practical network setup.

---

## 🌐 Topology Overview

- **Switch SW1** connecting VLAN10 and VLAN30 PCs.
- **Switch SW2** connecting VLAN10 and VLAN20 PCs.
- **Router R1** providing inter-VLAN routing for all VLANs.

**VLANs**:

- VLAN10 – Engineering
- VLAN20 – HR
- VLAN30 – Sales

Each PC is assigned to a specific VLAN and configured with an IP address and default gateway. Switches are connected via trunk links to carry multiple VLAN traffic.

---

## ⚙️ Configuration Steps

1. Configure switch access ports for PCs:

   - Assign each port to its VLAN (VLAN10, VLAN20, VLAN30).  
   - Verify VLANs are created and ports are assigned correctly.

2. Configure trunk ports between SW1 and SW2:

   - Enable trunking on the uplink interface.  
   - Allow only required VLANs (VLAN10, VLAN30).  
   - Set unused VLAN as native VLAN (VLAN1001).  
   - Verify trunk configuration and allowed VLANs.

3. Configure trunk between SW2 and R1:

   - Allow all VLANs (VLAN10, VLAN20, VLAN30).  
   - Set native VLAN to unused VLAN (VLAN1001).  
   - Verify trunk configuration.

4. Configure router sub-interfaces for inter-VLAN routing:

   - Enable physical interface.  
   - Create sub-interfaces for each VLAN.  
   - Assign IP addresses (typically last usable IP of each subnet).  
   - Enable `no shutdown` on each sub-interface.

5. Test connectivity:

   - Ping between PCs on the same VLAN.  
   - Ping across VLANs to ensure inter-VLAN routing works.

---

## 📝 Commands Used

```bash
# SW1 Access Ports
enable
configure terminal
interface range f0/1 - 2
switchport mode access
switchport access vlan 10

interface range f0/3 - 4
switchport mode access
switchport access vlan 30

# SW2 Access Ports
enable
configure terminal
interface f0/1
switchport mode access
switchport access vlan 20

interface range f0/2 - 3
switchport mode access
switchport access vlan 10

# SW1 Trunk
interface g0/1
switchport mode trunk
switchport trunk allowed vlan 10,30
switchport trunk native vlan 1001

# SW2 Trunk
interface g0/1
switchport mode trunk
switchport trunk allowed vlan 10,30
switchport trunk native vlan 1001

# SW2 to Router Trunk
interface g0/2
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport trunk native vlan 1001

# Router Sub-interfaces
enable
configure terminal
interface g0/0
no shutdown

interface g0/0.10
encapsulation dot1Q 10
ip address 10.0.0.62 255.255.255.192

interface g0/0.20
encapsulation dot1Q 20
ip address 10.0.0.126 255.255.255.192

interface g0/0.30
encapsulation dot1Q 30
ip address 10.0.0.190 255.255.255.192

```

🧠 Technical Explanation

This lab teaches:

How to segment a network using VLANs to improve security and reduce broadcast domains.

How access ports restrict switch interfaces to a single VLAN.

How trunk ports carry multiple VLANs over a single physical link.

Importance of inter-VLAN routing through router sub-interfaces.

How to assign IP addresses, subnet masks, and default gateways for proper communication.

Using show commands and ping to verify network functionality.

---

🌍 Real-World Use Case

Segmentation of department networks (Engineering, HR, Sales) in enterprises.

Reducing unnecessary broadcast traffic and improving performance.

Enabling secure inter-department communication via routers.

Troubleshooting and verifying VLAN and routing configurations in corporate networks.

---

🛠️ Skills Gained

VLAN creation and management

Configuring switch access and trunk ports

Router sub-interface configuration for inter-VLAN routing

IP addressing and subnetting

Network testing using CLI commands

Basic troubleshooting of VLAN connectivity

---

⚡ Possible Improvements

Use dynamic VLAN assignment with VLAN Membership Policy Server (VMPS).

Enable port security and disable unused ports for safety.

Integrate DHCP for automatic IP assignment.

Add network diagrams for visual reference.

Implement redundancy using STP or EtherChannel on trunk links.

---

🧩 Troubleshooting Notes

Verify VLAN assignment: show vlan brief

Check trunk links: show interfaces trunk

Confirm router sub-interface IPs match PC gateways

Ensure all interfaces are no shutdown

Use ping to test connectivity within and across VLANs

Verify cabling types (straight-through for router-switch, switch-PC connections)
