![Topology Diagram](https://github.com/furkangurses/STP-Load-Balancing-and-Port-Security-Lab-PVST-/blob/main/1345.PNG?raw=true)

# STP Load Balancing and Port Security Lab (PVST+)

---

## 🎯 Lab Objective

This lab demonstrates how to:

- Analyze default STP behavior
- Identify root bridge and port roles
- Configure per-VLAN root bridge placement (PVST+)
- Implement STP load balancing across VLANs
- Manipulate STP cost and port priority
- Configure PortFast and BPDU Guard
- Observe real-time STP behavior changes

---

## 🌐 Topology Overview

**Devices Used:**

- 4x Layer 2 Switches (SW1, SW2, SW3, SW4)
- 1x PC (PC1)
- All links are FastEthernet

**VLANs:**

- VLAN 1
- VLAN 2

**Initial State:**

- STP enabled by default (PVST+)
- SW2 elected as root bridge for VLAN 1 and VLAN 2
- Same STP topology for both VLANs

**Goal:**

- SW1 → Root Primary for VLAN 1 / Secondary for VLAN 2  
- SW2 → Root Primary for VLAN 2 / Secondary for VLAN 1  
- Achieve traffic load balancing across VLANs

---

## ⚙️ Configuration Steps

### 1️⃣ Verify Initial STP Topology

- Checked root bridge
- Identified root ports
- Verified designated and alternate ports

Confirmed:
- SW2 was root for both VLANs
- Other switches selected root ports based on lowest path cost

---

### 2️⃣ Configure STP Load Balancing (Per VLAN Root)

**On SW1:**
- VLAN 1 → Root Primary
- VLAN 2 → Root Secondary

**On SW2:**
- VLAN 1 → Root Secondary
- VLAN 2 → Root Primary

Result:
- VLAN 1 traffic optimized toward SW1
- VLAN 2 traffic optimized toward SW2
- Per-VLAN load balancing achieved

---

### 3️⃣ Modify STP Interface Cost

On SW4:

- Increased VLAN 1 cost on F0/2 to 100
- Forced root port re-election

Result:
- Root port changed from F0/2 → F0/1
- Demonstrated cost-based root port selection

---

### 4️⃣ Modify STP Port Priority

On SW1:

- Changed VLAN 1 port priority of F0/1 to 240

Result:
- No root port change on SW3
- Confirmed port priority is a tiebreaker (after cost and bridge ID)

---

### 5️⃣ Configure PortFast and BPDU Guard

On SW3 and SW4 (access ports connected to PC):

- Enabled PortFast
- Enabled BPDU Guard

Test Results:

- PortFast → Interface transitioned immediately to forwarding
- BPDU Guard → Interface shut down upon receiving BPDU
- Manual recovery required using shutdown / no shutdown

---

## 📝 Commands Used

```bash
# Verify STP
show spanning-tree
show spanning-tree vlan 1
show spanning-tree vlan 2

# Configure Root Bridge
conf t
spanning-tree vlan 1 root primary
spanning-tree vlan 2 root secondary

spanning-tree vlan 1 root secondary
spanning-tree vlan 2 root primary

# Modify Interface Cost
interface f0/2
spanning-tree vlan 1 cost 100

# Modify Port Priority
interface f0/1
spanning-tree vlan 1 port-priority 240

# Configure PortFast and BPDU Guard
interface f0/3
spanning-tree portfast
spanning-tree bpduguard enable

# Recover Disabled Interface
shutdown
no shutdown
```

---

## 🧠 Technical Explanation

This lab demonstrates how Cisco PVST+ operates on a per-VLAN basis.

Key concepts:

- STP elects a root bridge based on lowest Bridge ID.
- Root ports are selected based on:
  1. Lowest path cost
  2. Lowest sender bridge ID
  3. Lowest sender port ID (tie-breaker)
- Interface cost directly influences path selection.
- Port priority only affects selection if cost is equal.
- PortFast bypasses Listening and Learning states.
- BPDU Guard protects edge ports from loops by disabling the interface upon BPDU reception.

This lab reinforces deterministic STP design instead of relying on default elections.

---

## 🌍 Real-World Use Case

In enterprise networks:

- Core/distribution switches are manually configured as STP roots.
- Different VLANs use different root bridges to distribute traffic load.
- PortFast is enabled on access ports to speed up endpoint connectivity.
- BPDU Guard prevents accidental switch connections by end users.

This configuration is standard in campus network deployments.

---

## 🛠️ Skills Gained

- STP root bridge analysis
- PVST+ per-VLAN configuration
- STP load balancing design
- Root port election mechanics
- Interface cost manipulation
- Port priority behavior understanding
- Layer 2 loop prevention techniques
- PortFast & BPDU Guard implementation
- Troubleshooting STP states

---

## ⚡ Possible Improvements

- Implement Rapid PVST+ and compare convergence times
- Add EtherChannel and observe STP behavior
- Introduce a Layer 3 switch for inter-VLAN routing
- Simulate link failures to observe reconvergence
- Configure Root Guard and Loop Guard for advanced protection

---

## 🧩 Troubleshooting Notes

- If root bridge does not change, verify bridge priority with:
  ```bash
  show spanning-tree vlan 1
  ```

- If interface remains disabled after BPDU Guard trigger:
  ```bash
  shutdown
  no shutdown
  ```

- If unexpected root port selection occurs:
  - Check interface cost
  - Verify port priority
  - Confirm VLAN-specific STP configuration

- Always verify STP per VLAN (PVST+ is VLAN-based)
