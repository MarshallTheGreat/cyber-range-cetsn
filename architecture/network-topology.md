# Overview

I created this cyber range as a fully virtualized enterprise and operational network design to replicate a contested cyber environment.

It models a layered, segmented infrastructure where attackers must pivot through multiple trust boundaries to reach mission-critical assets.

The environment supports:
  - Defensive monitoring and detection
  - Adversarial lateral network movement
  - Mission assurance and MRT-C analysis
  - Both red and blue team training

The topology enforces realistic enterprise constraints including segmented networks, identity boundaries, logging choke points, and limited administrative trust.


# High-Level Architecture

The environment is divided into three primary zones (blue and red could switch depending on the training occuring at the time):
  - Blue Network - Operator workstation
  - Gray Network - Intermediary pivot infrastructure
  - Red Network - Target enterprise environment

Traffic flow is intentionally constrained to force the use of SSH tunneling and socket forwarding to progress deeper into the environment.


[Blue Box]
|
| - - - (SSH Tunnel) - - > [Gray Box]                                                 | - - -> [HQ Command]
|                           |                                                         |
                            | - - - (Tunnel Socket) - - -> [Red Gateway Router] - - - | - - -> [Intel Ops] - - - - - - - | - - - > [Defense Node] - - - > [Recon Unit] - - - > [Cyber Intel] - - - > [Communications]
                            |                                                         |                                  |
                                                                                      | - - -> [Surveillance Unit] - - - |

## Network Flow

### 1. Blue Box > Gray Box
The operator initiates access from the **Blue Box** using outbound SSH tunneling.  
No inbound services are exposed, simulating a hardened operator workstation.

### 2. Gray Box > Red Gateway Box
The **Gray Box** acts as a controlled pivot point.  
Tunnel sockets forward traffic into the Red Gateway Box, enforcing a single choke point.

### 3. Red Gateway Box > Red Network
The **Red Gateway Box** provides routed access into the Red Network where mission systems reside.


## Component Breakdown

### Blue Box
**Role:** Operator Workstation  
**Function:** Launch point for cyber operations

#### Services
- SSH client
- Network analysis tools (tcpdump, Wireshark)

#### Network Characteristics
- No inbound ports exposed
- Outbound SSH only

#### Primary Risk Areas
- SSH key handling
- Client-side misconfiguration
--------------------------------------------
### Gray Box
**Role:** Intermediary Pivot Host  
**Function:** Tunnel termination and traffic forwarding

#### Services
- SSH server
- SOCKS proxy via SSH dynamic port forwarding

#### Open Ports
- TCP 22 (SSH)
- TCP 8080 (Proxy)

#### Primary Risk Areas
- Weak SSH configuration
- Proxy misconfiguration
--------------------------------------------
### Red Gateway Box
**Role:** Red Network Entry Point  
**Function:** Routing and access control

#### Services
- Linux kernel IP forwarding
- Static routing
- iptables (network segmentation and access control)

#### Open Ports
- TCP 22 (SSH)
- TCP 80 (Administrative access)

#### Primary Risk Areas
- Weak authentication
- Unpatched admin interfaces
