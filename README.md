# Layer 2 Resilience: STP & RSTP Optimization Lab

## 📌 Project Overview
This project demonstrates the implementation, optimization, and troubleshooting of Spanning Tree Protocol (STP) and Rapid-PVST+ in a redundant Layer 2 topology. The goal is to ensure loop-free networks while achieving sub-second convergence times and enforcing strict edge security.

## 🏗️ Topology
- **3x Cisco IOSv-L2 Switches** (LAB-SW-01, LAB-SW-02, LAB-SW-03)
- **Triangle Topology** with redundant GigabitEthernet links to simulate potential Layer 2 loops.
- **LAB-SW-01:** Primary Root Bridge
- **LAB-SW-02:** Secondary Root Bridge
- **LAB-SW-03:** Access Switch with Edge Security features.

## 🛠️ Prerequisites
- EVE-NG or GNS3 environment.
- Cisco IOSv-L2 or IOL Layer 2 images.

## ⚙️ Configuration Highlights

### 1. Root Bridge Manipulation
Forcing predictable Root Bridge election instead of relying on default MAC address comparisons.
```cisco
! On LAB-SW-01
spanning-tree vlan 1 root primary

! On LAB-SW-02
spanning-tree vlan 1 root secondary

2. Rapid-PVST+ Migration
Upgrading from legacy 802.1D to 802.1w for sub-second convergence.

spanning-tree mode rapid-pvst

3. Edge Port Security (PortFast & BPDU Guard)
Applied to user-facing ports (e.g., Gi0/0 on LAB-SW-03) to bypass listening/learning states and immediately shut down the port if an unauthorized switch sends BPDUs.

interface GigabitEthernet0/0
 spanning-tree portfast
 spanning-tree bpduguard enable

🔍 Verification & Troubleshooting
show spanning-tree vlan 1: Verify Root Bridge, Root Ports, and Alternate (Blocking) ports.
show spanning-tree interface [interface] detail: Confirm PortFast and BPDU Guard status.
show spanning-tree summary: Validate global Rapid-PVST mode and guard statistics.
Failover Test: Shutting down the Root Port to observe instant convergence to the Alternate port.

🛡️ Production Best Practices Implemented
Manual Root Bridge placement.
BPDU Guard on all edge ports.
(Recommended for production) Root Guard on uplinks and Loop Guard on point-to-point fiber links.
