# Lab 3.1 Segmentation 1

## Overview
This covers the key concepts and procedures implemented in our network segmentation and security lab. The lab focuses on enhancing network security through segmentation, firewall configuration, and system management.

## Network Segmentation

### Purpose
Network segmentation is a critical security practice that involves dividing a network into smaller subnetworks. This approach:

- Improves security by isolating different parts of the network
- Enhances performance by reducing network congestion
- Facilitates better network management and troubleshooting

### Implementation
In this lab, the network is segmented into three main parts:

1. **LAN (SEC-350-LAN)**: 172.16.150.0/24
2. **DMZ**: (Not specified in the lab, but typically used for public-facing services)
3. **MGMT (Management)**: 172.16.200.0/28

## Firewall Configuration

### Firewall Devices
Two main firewall devices are configured:

1. **fw01**: Primary firewall
2. **fw-mgmt**: Management firewall

### Key Configurations

#### NAT (Network Address Translation)
NAT is configured on fw01 with the following rules:
- Rule 10: NAT for DMZ
- Rule 20: NAT for LAN
- Rule 30: NAT from MGMT to WAN

#### Interface Configuration
- **fw01**:
  - LAN interface: 172.16.150.2/24
- **fw-mgmt**:
  - eth0 (LAN): 172.16.150.3/24
  - eth1 (MGMT): 172.16.200.2/28

#### DNS Forwarding
Configured on fw-mgmt to allow requests from the management subnet and interface.

#### Routing
Static routes and RIP (Routing Information Protocol) are configured to ensure proper communication between network segments.

## System Configuration

### Windows Systems

#### WKS01 (Workstation)
- OS: Windows 10
- Network: SEC-350-LAN
- IP: 172.16.150.50/24
- Gateway: 172.16.150.2
- DNS: 172.16.150.2

#### MGMT02 (Management Server)
- OS: Windows Server
- Network: SEC350-MGMT
- IP: 172.16.200.11/28
- Gateway: 172.16.200.2
- DNS: 172.16.200.2

### Linux Systems

#### Wazuh (Security Information and Event Management)
- OS: Ubuntu Server
- Network: SEC350-MGMT
- IP: 172.16.200.10/28
- Gateway: 172.16.200.2
- DNS: 172.16.200.2

## Network Protocols and Services

### RIP (Routing Information Protocol)
RIP is configured on both fw01 and fw-mgmt to advertise their respective networks:
- fw01 advertises the DMZ network
- fw-mgmt advertises the MGMT network

### DNS (Domain Name System)
DNS services are provided by the firewalls for their respective network segments.

### SSH (Secure Shell)
Used for secure remote administration of systems.

## Security Practices

1. **Least Privilege**: Administrative access is limited to the management network.
2. **Network Isolation**: The management network is separated from the production LAN.
3. **Centralized Logging**: Wazuh is implemented for centralized log management and security monitoring.

## Configuration Management

### VyOS Firewall Configuration Export
To maintain consistent and recoverable configurations, firewall settings are exported using the command line. This practice ensures:

- Version control of firewall rules
- Easy rollback in case of misconfigurations
- Documentation of network security policies

## Troubleshooting and Validation

Several validation steps are performed to ensure proper network functionality:

1. Ping tests between different network segments
2. Traceroute to verify routing paths
3. SSH connectivity tests
4. Web server access tests

These tests help confirm that:
- Network segmentation is working as intended
- Routing between segments is functional
- Security policies are correctly implemented
- Internet access is available where required

By implementing these concepts and practices, the lab demonstrates a comprehensive approach to network segmentation and security, providing a foundation for a robust and secure network infrastructure.

Citations:
[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/44386984/fe8f7208-2c08-4e84-a915-994f47a0607f/SEC-350-Lab-3.1-Segmentation.docx
