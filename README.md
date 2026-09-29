# Secure Enterprise Network Topology with Cisco Packet Tracer

![Cisco](https://img.shields.io/badge/Cisco-Packet_Tracer-049fd9?style=for-the-badge&logo=cisco)
![Networking](https://img.shields.io/badge/Networking-Routing_%26_Switching-blue?style=for-the-badge)
![Security](https://img.shields.io/badge/Security-ACLs-red?style=for-the-badge)

## 📖 Project Overview
This project demonstrates the design and implementation of a secure multi-site corporate network using Cisco Packet Tracer. The core objective is to establish a robust enterprise topology with a dedicated Demilitarized Zone (DMZ), ensuring internal network isolation through the deployment of Cisco IOS security features, network segmentation, and static routing.

## 🏗 Network Topology Structure
The network architecture represents a typical enterprise setup connecting a Head Office (HQ) to a Branch Office via an Internet Service Provider (ISP). 

### 1. Head Office (HQ)
- **Router:** Cisco ISR 4321 (equipped with a NIM-2T serial module for WAN connectivity).
- **Internal Segmentation:**
  - **HQ Internal LAN (Employees):** `192.168.10.0/24` - Isolated from external access.
  - **HQ DMZ (Web & AAA Servers):** `192.168.20.0/24` - Accessible from the outside to host public-facing services.

### 2. Branch Office
- **Router:** Cisco ISR 4321.
- **Branch LAN (Employees):** `192.168.30.0/24` - Represents remote corporate users requiring secure connectivity.

### 3. ISP Router
- Simulates the WAN/Internet cloud providing connectivity between the HQ and the Branch office.

### 4. WAN Links (Point-to-Point)
- **HQ to ISP:** Serial Connection (`10.10.10.0/30`)
- **Branch to ISP:** Gigabit Ethernet Connection (`10.10.20.0/30`)

## 🖥️ IP Addressing Table

| Device / Location | Interface / Segment | Network Address | Subnet Mask | Description |
| :--- | :--- | :--- | :--- | :--- |
| **HQ Router** | Internal LAN | `192.168.10.0` | `255.255.255.0` (`/24`) | Employee Network |
| **HQ Router** | DMZ | `192.168.20.0` | `255.255.255.0` (`/24`) | Public-facing Servers |
| **Branch Router** | Branch LAN | `192.168.30.0` | `255.255.255.0` (`/24`) | Remote Employees |
| **HQ - ISP** | Serial WAN Link | `10.10.10.0` | `255.255.255.252` (`/30`) | P2P Serial Connection |
| **Branch - ISP** | Gigabit WAN Link | `10.10.20.0` | `255.255.255.252` (`/30`) | P2P Gigabit Connection |

## ⚙️ Technologies & Protocols Implemented
- **Standardized IPv4 Addressing:** Efficient subnetting using VLSM for point-to-point links (`/30`).
- **Static Routing:** 
  - Default routes (`ip route 0.0.0.0 0.0.0.0`) configured on edge routers (HQ & Branch) pointing to the ISP.
  - Explicit static routes configured on the ISP router to reach internal subnets.
- **Network Segmentation:** Physical and logical separation of employee traffic from server (DMZ) traffic to reduce the attack surface.

## 🔒 Security Implementation (Extended ACLs)
A critical security requirement for this topology is to protect the internal employee network while keeping DMZ services available to the outside world. 

To achieve this, an **Inbound Extended Access Control List (ACL)** is applied to the WAN-facing interface of the HQ router. The logic of the ACL is as follows:
1. **DENY** any external traffic destined for the HQ Internal LAN (`192.168.10.0/24`). This ensures absolute isolation of employee workstations from the Internet.
2. **PERMIT** external traffic destined for the HQ DMZ (`192.168.20.0/24`). This allows external users (e.g., from the Branch Office) to access the Web and AAA servers hosted in the DMZ.
3. **Implicit Deny:** All other unauthorized traffic is dropped by default.

## 🚀 How to Run
1. Ensure you have [Cisco Packet Tracer](https://skillsforall.com/course/getting-started-cisco-packet-tracer) installed on your machine.
2. Clone or download this repository.
3. Locate the `.pkt` simulation file in the repository folder.
4. Double-click the file to open it in Cisco Packet Tracer.
5. You can test connectivity and security policies by:
   - Pinging from the Branch LAN to the HQ DMZ (Should succeed).
   - Pinging from the Branch LAN to the HQ Internal LAN (Should fail, blocked by ACL).
