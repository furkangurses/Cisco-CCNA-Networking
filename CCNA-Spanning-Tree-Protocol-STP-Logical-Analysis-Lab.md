![Topology Diagram](https://github.com/furkangurses/CCNA-Spanning-Tree-Protocol-STP-Logical-Analysis-Lab/blob/main/4345.PNG?raw=true)

# CCNA Spanning Tree Protocol (STP) Logical Analysis Lab

---

## 🎯 Lab Objective
To develop a deep, protocol-level understanding of **Spanning Tree Protocol (STP)** by manually analyzing the topology, calculating STP decisions, and verifying results using CLI commands in Cisco Packet Tracer.

This lab focuses on **logical reasoning**, not configuration automation.

---

## 🌐 Topology Overview
- Multi-switch Layer 2 topology
- Single VLAN environment (VLAN 1)
- Mixed FastEthernet and GigabitEthernet links
- Cisco PVST (classic IEEE 802.1D STP mode)
- Root Bridge election based on Bridge ID (Priority + MAC)
- Manual port role calculation:
  - Root Port
  - Designated Port
  - Alternate (Blocking) Port

Topology consists of:
- SW1
- SW2
- SW3 (Root Bridge)
- SW4

---

## ⚙️ Configuration Steps
1. Disable **link lights** in Packet Tracer to avoid visual hints:
   - Options → Preferences → Disable **"Show Link Lights"**
2. Analyze topology manually:
   - Compare Bridge Priorities
   - Identify Root Bridge
   - Calculate Root Path Cost
   - Apply STP decision rules
3. Determine port roles:
   - Root Ports
   - Designated Ports
   - Alternate (Blocking) Ports
4. Validate all decisions using CLI commands.

---

## 📝 Commands Used
```bash
enable
show spanning-tree
show spanning-tree vlan 1
show spanning-tree detail
show spanning-tree summary

```

## 🧠 Technical Explanation
This lab teaches **how STP actually works internally**, not just how to configure it.

Key protocol logic demonstrated:

### Root Bridge Election
- Lowest Bridge Priority wins  
- If equal → Lowest MAC Address wins  
- Result: **SW3 becomes Root Bridge (Priority: 24577)**

### Root Port Selection Criteria (in order)
1. Lowest Root Path Cost  
2. Lowest Neighbor Bridge ID  
3. Lowest Neighbor Port ID  

### Designated Port Selection
- Interface on the switch with **lower Root Path Cost** wins  
- Other side becomes **Alternate (Blocking)**

### Protocol Mode
- Cisco PVST  
- Classic IEEE 802.1D STP behavior  
- Single-VLAN spanning tree  

This lab enforces **algorithmic decision-making**, not memorization.

---

## 🌍 Real-World Use Case
- Enterprise campus networks  
- Redundant Layer 2 switching environments  
- Loop prevention design  
- Network stability planning  
- STP troubleshooting in production networks  
- Switch interconnection design validation  

---

## 🛠️ Skills Gained
- STP protocol analysis  
- Layer 2 topology reasoning  
- Root bridge planning  
- Root path cost calculation  
- Tie-breaker logic application  
- CLI verification techniques  
- Network troubleshooting methodology  
- Protocol-based decision modeling  
- Cisco CLI interpretation  
- Network design validation  

---

## ⚡ Possible Improvements
- Add Rapid Spanning Tree Protocol (RSTP) comparison  
- Multi-VLAN STP (PVST+) implementation  
- Root bridge manipulation using priority configuration  
- STP timer analysis (Hello, Forward Delay, Max Age)  
- Failure simulation scenarios  
- Link failure convergence testing  
- STP optimization design scenarios  

---

## 🧩 Troubleshooting Notes
- Always verify Root Bridge using:
  - `show spanning-tree`
- Do not trust visual topology indicators  
- Link lights can bias analysis  
- Always validate **Root Path Cost**, not interface cost  
- Distinguish between:
  - Interface cost  
  - Total root path cost  
- Remember:
  - Root Bridge → all ports are Designated  
  - Each non-root switch → only one Root Port  
- Tie-breakers must follow correct priority order:
  1. Cost  
  2. Bridge ID  
  3. Port ID  
