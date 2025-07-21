# NET3207: Network Management - LAN Configuration Project


This repository contains the documentation and final report for the NET3207 Network Management coursework. This group project involved the design, configuration, and integration of a multi-service Local Area Network (LAN) using a variety of physical hardware and networking technologies.

## 👥 Team Members

*   Suhail Dorasamy
*   Nidal Bencheikh Lehocine
*   Elena Khoo Sze Kay
*   Dharshaan A/L Segaran
*   Tasha Hong Ruen Lin
*   Abdul Rahman Fedda

## 🚀 Project Overview

The primary objective of this project was to configure a complex LAN environment that incorporates multiple VLANs to segment network traffic. The setup includes a range of network components, such as a PBX server for VoIP communication, a web server for content hosting, a CCTV camera for surveillance, and a MikroTik router providing internet access via a 3G USB dongle.

The network was designed to allow seamless inter-VLAN communication, ensuring that a central workstation could ping all devices, access the internet, and reach the hosted web server.

### Network Topology

The network is structured across three distinct VLANs to logically separate services. A trunk link was established between the Cisco Router and Switch to facilitate inter-VLAN routing.


*(Image from the project report, Figure 1)*

### VLAN & Subnet Design

| VLAN ID | Name      | Purpose                               | Network Address         | Gateway             |
| :------ | :-------- | :------------------------------------ | :---------------------- | :------------------ |
| **100** | WebServer | Hosts the Web Server                  | `192.168.13.104/29`     | `192.168.13.105`    |
| **200** | VoIP      | Hosts the PBX Server and IP Phones    | `192.168.13.112/29`     | `192.168.13.113`    |
| **300** | Services  | Hosts the CCTV and MikroTik Router    | `192.168.13.120/29`     | `192.168.13.121`    |

---

## 💻 Core Technologies & Hardware

-   **Routers:** Cisco 1941, MikroTik 960
-   **Switch:** Cisco Catalyst 2960 Plus
-   **PBX System:** Yeastar MyPBX SOHO Server
-   **IP Phones:** Yealink SIP-T20P
-   **Web Server:** Ubuntu Server with Apache2
-   **CCTV:** Grandstream GXV3611IR_HD Camera
-   **Internet Source:** 3G/LTE USB Dongle
-   **Key Protocols/Concepts:** VLANs (802.1Q), Inter-VLAN Routing (Router-on-a-Stick), DHCP, NAT (Masquerade), VoIP (SIP), HTTP.

---

## 🛠️ Configuration Details

This section summarizes the key configuration steps for each major component of the network.

### 1. Cisco Router (DHCP & Inter-VLAN Routing)

-   Configured as a **Router-on-a-Stick** to handle traffic between VLANs.
-   The physical `GigabitEthernet0/0` interface was configured as a trunk link.
-   Created **sub-interfaces** (`.100`, `.200`, `.300`) for each VLAN, each with `encapsulation dot1Q`.
-   Served as the **DHCP server** for all VLANs, with separate pools for each.
-   IP addresses for gateways and critical servers were excluded from the DHCP pools using `ip dhcp excluded-address`.
-   A default route (`0.0.0.0 0.0.0.0`) was added, pointing to the MikroTik router (`192.168.13.125`) for internet access.

### 2. Cisco Switch (VLAN & Port Configuration)

-   Cleared previous configurations (`erase startup-config`, `delete vlan.dat`) to ensure a clean slate.
-   Created VLANs 100, 200, and 300.
-   Assigned switch ports to their respective VLANs in **access mode** (`switchport mode access`).
-   Configured the port connected to the Cisco Router (`f0/24`) as a **trunk port** (`switchport mode trunk`) to carry traffic for all VLANs.

### 3. PBX Server & VoIP Phones (Yeastar & Yealink)

-   The Yeastar MyPBX server was configured with a static IP (`192.168.13.116`) within VLAN 200.
-   Two extensions (1001, 1002) were created for the two IP phones.
-   The **"Register Remotely"** option was enabled to allow IP phones from different VLANs to connect.
-   IP phones were configured to use the PBX server's IP for **auto-provisioning**.
-   **Challenge:** The IP phone in VLAN 100 did not register automatically. **Workaround:** A manual reboot while unplugging/replugging the Ethernet cable was required to force the phone to acquire its configuration.

### 4. Web Server (Ubuntu/Apache)

-   Hosted on an Ubuntu virtual machine within VLAN 100.
-   The **Apache2** service was installed and started.
-   Two simple HTML files (`group13.html`, `topology.html`) were created and placed in `/var/www/html`.
-   The VM's network adapter was set to **Bridged Mode** to connect to the physical network.
-   The server successfully received an IP address from the Cisco router's DHCP pool for VLAN 100.
-   The website was accessible from the workstation in VLAN 200, demonstrating successful inter-VLAN routing.

### 5. CCTV Camera (Grandstream)

-   Initially configured by directly connecting it to a workstation.
-   The **"gs_search"** tool was used to discover the camera's default IP.
-   The camera was assigned a static IP (`192.168.13.124`) within VLAN 300.
-   **Challenge:** The video feed did not display in modern browsers. **Workaround:** Internet Explorer compatibility mode had to be enabled in Microsoft Edge, and the camera's GUI URL was added to the trusted sites list.

### 6. MikroTik Router (Internet Gateway & NAT)

-   Connected to the internet via a 3G USB Dongle, which appeared as the `lte1` interface.
-   Configured using the **WinBox** application.
-   **Challenge:** The router's default configuration interfered with custom setup. **Workaround:** The default configuration was explicitly removed using the prompt that appears after a factory reset.
-   A **NAT (masquerade)** rule was created on the `lte1` interface to allow all devices on the LAN to share the public IP address for internet access.
-   Static routes were added to allow the MikroTik router (in VLAN 300) to reach the other VLANs (100 & 200) via the Cisco router's gateway IPs.

---

## 🧠 Key Learnings

-   **Teamwork and Collaboration:** Dividing tasks based on individual strengths was crucial for managing the complexity of the project.
-   **Systematic Troubleshooting:** When issues arose (like the IP phone registration or MikroTik NAT), a step-by-step, patient approach was necessary to diagnose and resolve the problem.
-   **Importance of Documentation:** Keeping detailed records of configurations made it easier to troubleshoot and ensure consistency across the network.
-   **Bridging Theory and Practice:** This hands-on project was invaluable for translating theoretical knowledge from lectures and simulations (like Packet Tracer) into a real-world, physical environment.
-   **Adapting to Niche Tools:** Successfully using vendor-specific tools like WinBox and gs_search was a key part of the implementation.

---

## 📂 Repository Structure

```
.
├── README.md                 <-- You are here
└── NET3207_Project_Report.pdf  <-- The complete project report with detailed steps and screenshots
```
