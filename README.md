# FortiGate Enterprise Security Lab

A simulated enterprise network security implementation using FortiGate
Next-Generation Firewall.

This project demonstrates how a FortiGate firewall can be used to protect
an enterprise environment through network segmentation, application-aware
firewall policies, security profiles, secure internet access, DMZ isolation,
logging, and high availability.

> **Disclaimer**
>
> This repository is an independent lab project created for educational and
> portfolio purposes. All organization names, domain names, IP addresses,
> usernames, diagrams, and configurations are fictional or sanitized.
>
> This project is not affiliated with any current or previous employer.
> No production configuration, credential, confidential information, or
> customer data is included.

---

## Project Objectives

The objectives of this lab are:

- Design a segmented enterprise network
- Separate users, servers, guests, management, and DMZ systems
- Implement least-privilege firewall policies
- Control internet access using security profiles
- Publish a dummy web application securely
- Isolate public-facing systems from internal servers
- Simulate redundant internet connectivity
- Demonstrate FortiGate high availability
- Centralize security event and traffic logging
- Document firewall implementation and validation procedures

---

## Scope

This lab covers the following areas:

- FortiGate initial configuration
- Interface and VLAN configuration
- Security zones
- Static and default routing
- Firewall address objects
- Firewall service objects
- IPv4 firewall policies
- Source NAT
- Destination NAT using Virtual IP
- Web filtering
- Application control
- Intrusion Prevention System
- Antivirus scanning
- DNS filtering
- SSL inspection
- Administrative access hardening
- Logging and monitoring
- High availability design
- Configuration backup and validation

The following components are outside the current scope:

- Production deployment
- FortiManager implementation
- FortiAnalyzer implementation
- Full Active Directory integration
- Remote-access VPN
- Site-to-site IPsec VPN
- Zero Trust Network Access
- Secure SD-WAN performance testing

These components may be added in future lab versions.

---

## Fictional Company Scenario

`Example Manufacturing Ltd.` is a fictional company with approximately
500 employees.

The company operates:

- A primary office
- Internal business applications
- Microsoft Active Directory and DNS servers
- A public-facing customer portal
- Corporate wireless access
- Guest wireless access
- Network management systems
- Two internet service providers

The company requires network segmentation and controlled communication
between different security zones.

---

## Logical Architecture
