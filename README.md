# 🌐 Enterprise Network Security & IoT Segmentation Project

![Cisco](https://img.shields.io/badge/Cisco-Packet_Tracer-blue?style=for-the-badge&logo=cisco)
![Firewall](https://img.shields.io/badge/Security-ASA_5505-red?style=for-the-badge)
![Routing](https://img.shields.io/badge/Routing-ROAS-green?style=for-the-badge)

## 📖 Overview
This project implements a **secure, enterprise-grade network infrastructure** using Cisco Packet Tracer. The network is designed to demonstrate advanced Layer 2 and Layer 3 segmentation, stateful firewall protection, IoT integration, and secure remote access. 

The network is structured to enforce strict security policies through VLAN isolation, Cisco Port Security, Access Control Lists (ACLs), and Dynamic NAT/PAT, simulating a real-world corporate environment.

## 🏗️ Network Architecture

---
<img width="1872" height="711" alt="topology_full" src="https://github.com/user-attachments/assets/8989d6b7-0bac-4166-a626-61c228194d67" /> 

---


### Core Topology Components:
*   **Core Switch:** Cisco Catalyst 2960 handling Layer 2 segmentation.
*   **Router-on-a-Stick (ROAS):** Cisco ISR 4321 providing inter-VLAN routing and DHCP services.
*   **Firewall:** Cisco ASA 5505 providing stateful packet inspection, NAT, and VPN termination.
*   **ISP Router:** Simulating the public internet and acting as the external gateway.
*   **End Devices:** Admin PCs, User PCs, Corporate Servers (Web, Mail, File, DNS), and various IoT devices (Webcam, Thermostat).

## 🔧 Technical Highlights & Configurations

### 1. VLAN Segmentation & Security
We utilized VLANs to strictly separate traffic types, enhancing both performance and security.
*   **VLAN 10 (Admins):** Dedicated for administrative personnel.
*   **VLAN 20 (Users):** Dedicated for standard employees.
*   **VLAN 30 (Servers):** Hosts internal DNS, Web, and Mail servers.
*   **VLAN 40 (IoT):** Dedicated, isolated network for IoT devices (Webcam).
*   **VLAN 50 (WAN Gateway):** Transit VLAN for the IoT Home Gateway to reach the infrastructure.
*   **VLAN 99 (Management):** Used for out-of-band management and Native VLAN.
*   **Port Security:** Access ports are hardened with `switchport port-security maximum 1`, `sticky` MAC address learning, and `restrict` violation mode to prevent unauthorized device connections.
*   **Unused Ports:** All unused switch ports have been administratively shut down (`shutdown`) to prevent rogue access.
*   **Spanning Tree:** Configured with `portfast` and `bpduguard` default to prevent loop vulnerabilities on access ports.

### 2. Router-on-a-Stick (ROAS) & DHCP
The router uses subinterfaces (`.10`, `.20`, `.30`, `.40`, `.50`, `.99`) with 802.1Q encapsulation for inter-VLAN routing. DHCP pools are configured for all user VLANs, automatically assigning IPs and pointing to our internal DNS server (`192.168.30.100`) while excluding statically assigned gateways and servers.

### 3. Firewall & Security Policies (ASA 5505)
The Cisco ASA acts as the primary gateway to the internet. Configuration highlights include:
*   **Security Zones:** Inside (Level 100) and Outside (Level 0) interfaces.
*   **Dynamic NAT (PAT):** `object network PAT_POOL` translates private internal IPs to the ASA's public interface IP (`209.165.200.2`).
*   **ICMP Inspection:** Configured to allow ping packets to traverse the firewall statefully.
*   **Static NAT / Port Forwarding:** The internal Web Server (`192.168.30.1`) is securely exposed to the internet using a static NAT mapping to `209.165.200.3` (TCP Port 80).
*   **VPN Service:** The ASA is configured with an IPsec VPN gateway. Administrators can authenticate with credentials (`admin`) and securely connect to the internal network from the outside using pre-shared keys.

### 4. IoT Integration
IoT devices are placed on their dedicated VLAN 40. The "Home Gateway" acts as a NAT between the Wi-Fi-connected IoT devices (on the `192.168.1.0/24` LAN) and the enterprise VLAN 50 WAN link.

### 5. Internet Service Provider (ISP) Simulation
The ISP router is configured with a Loopback interface simulating `8.8.8.8` (Google DNS). It has a specific static route pointing the entire internal `192.168.0.0/16` network back to the ASA firewall (`209.165.200.2`), ensuring replies from the internet correctly reach the internal networks.

## 📂 Repository Structure

```
/
├── README.md
├── Images/                 # Screenshots of topology and connectivity tests
├── Network-Diagram/        # Packet Tracer .pkt file
└── Configs/                # Pure text files of device CLI configurations
    ├── CentralSwitch_Config.txt
    ├── ROAS_Router_Config.txt
    ├── ASA5505_Firewall_Config.txt
    └── ISP_Router_Config.txt
```

## 📈 Traffic Flow Validation

---

<img width="816" height="562" alt="ping_admin_to_isp" src="https://github.com/user-attachments/assets/24efb3ce-3ad6-4e14-88de-01db13230322" />

---

Internal to External: An Admin PC (192.168.10.1) successfully pings the ISP router (209.165.200.1) and the simulated Google DNS (8.8.8.8) via NAT.

IoT to External: The Webcam (IoT_Device1) successfully pings the ISP router (209.165.200.1).

External to Internal: An external browser successfully accesses the internal corporate web server via the public IP http://209.165.200.3.

## 👨‍💻 Author & Project Goals
This project was engineered to demonstrate core Network Engineering and Cybersecurity principles. It highlights the importance of Defense-in-Depth, utilizing physical port security, logical network segmentation, and stateful firewall filtering to create a robust enterprise environment.


*** Author: Mehdi Mejri ***

*** Organization: Tunisie Telecom ***

## Position: Intern

Feel free to explore the configurations in the Configs/ directory to view the exact CLI commands used for these security implementations.

<p align="center"> <sub>Built with ❤️ and Cisco Packet Tracer by <a href="https://github.com/Mejri-Mehdi">Mejri Mehdi</a></sub> </p>
