# Project Documentation

This directory contains the design, implementation, hardening, and validation
documentation for the FortiGate Enterprise Security Lab.

The documentation represents a fictional enterprise environment created for
educational and portfolio purposes.

---

## Available Documentation

### Firewall Policy Matrix

Defines approved communication between corporate users, internal servers,
guest devices, management systems, security-monitoring systems, the DMZ, and
the internet.

The document includes:

- Network and security-zone definitions
- Firewall address and service objects
- Security-profile assignments
- Detailed firewall policies
- NAT and Virtual IP requirements
- Logging requirements
- Policy validation procedures
- Troubleshooting commands

[View Firewall Policy Matrix](firewall-policy-matrix.md)

---

### Implementation Plan

Defines the phased implementation process for system settings, interfaces,
VLANs, security objects, SD-WAN, routing, application publishing, firewall
policies, management access, logging, and FortiGate High Availability.

The plan includes:

- Prerequisites
- Dependencies
- Implementation phases
- Validation checkpoints
- Rollback considerations
- Success criteria
- Post-implementation review requirements

[View Implementation Plan](implementation-plan.md)

---

### Security Hardening Checklist

Defines security-hardening requirements for administrator accounts,
management access, interfaces, firewall policies, UTM profiles, the DMZ,
guest access, SD-WAN, High Availability, centralized logging, firmware
management, and configuration backups.

The checklist includes:

- Administrator-account protection
- Trusted-host restrictions
- Management-plane isolation
- Interface hardening
- Firewall-policy review
- Security-profile validation
- DMZ and guest isolation
- Logging and monitoring controls
- Configuration-backup protection
- Firmware and vulnerability management

[View Security Hardening Checklist](hardening-checklist.md)

---

### Validation Test Plan

Defines functional, negative, security, and failover test cases for the
FortiGate Enterprise Security Lab.

The test plan covers:

- VLAN and network connectivity
- Inter-zone segmentation
- Corporate internet access
- Guest-network isolation
- Internal application access
- Database-access restrictions
- Destination NAT and Virtual IP
- Security-profile enforcement
- Administrative-access restrictions
- Centralized logging
- SD-WAN failover
- FortiGate High Availability

[View Validation Test Plan](test-plan.md)

---

## Planned Documentation

The following documents will be added during the next project phase:

- Validation test results
- Security risk register
- High-availability failover procedure
- SD-WAN failover procedure
- Rollback plan
- Troubleshooting guide
- Implementation evidence index

Documents that depend on test execution will remain marked as pending until
the relevant tests are completed in a controlled laboratory environment.

---

## Document Status

| Document | Status |
|---|---|
| Firewall Policy Matrix | Complete |
| Implementation Plan | Complete |
| Security Hardening Checklist | Complete |
| Validation Test Plan | Complete |
| Validation Test Results | Pending Lab Execution |
| Security Risk Register | Planned |
| HA Failover Procedure | Planned |
| SD-WAN Failover Procedure | Planned |
| Rollback Plan | Planned |
| Troubleshooting Guide | Planned |
| Implementation Evidence Index | Pending Lab Execution |

---

## Related Project Resources

- Main entation Principles

The project documentation follows these principles:

- Least-privilege access
- Default-deny security
- Defense in depth
- Network segmentation
- Management-plane isolation
- Controlled implementation
- Documented validation
- Configuration sanitization
- Evidence-based testing
- Clear separation between designed and tested controls

A control is not marked as successfully tested until it has been executed and
validated in a controlled lab environment.

---

## Disclaimer

All organizations, systems, IP addresses, domain names, usernames, and
configurations in this directory are fictional or sanitized.

No production configuration, credentials, private keys, certificates,
customer information, employer information, or confidential data is included.

The examples must be independently reviewed before use in any real
environment.
