# Project Documentation

This directory contains the design, implementation, hardening, risk,
validation, failover, rollback, and troubleshooting documentation for the
FortiGate Enterprise Security Lab.

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
policies, management access, centralized logging, and FortiGate High
Availability.

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

Defines functional, negative, security, connectivity, and failover test cases
for the FortiGate Enterprise Security Lab.

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

### Security Risk Register

Identifies and evaluates security and operational risks associated with
firewall policies, network segmentation, administrative access, the DMZ,
High Availability, SD-WAN, security profiles, centralized logging, firmware
management, configuration backups, and public repository sanitization.

The risk register includes:

- Qualitative risk-assessment methodology
- Likelihood and impact ratings
- Inherent and residual risk
- Existing security controls
- Additional mitigation actions
- Risk ownership
- Open risk-treatment actions
- Risk-review triggers

[View Security Risk Register](risk-register.md)

---

### High-Availability Failover Procedure

Defines controlled failover testing for the FortiGate Active-Passive High
Availability cluster.

The procedure includes:

- Pre-test cluster validation
- Controlled primary-member failover
- Monitored-interface failure
- Single heartbeat-link failure
- Configuration synchronization testing
- Failure and rollback criteria
- Post-failover validation
- Evidence requirements

[View HA Failover Procedure](ha-failover-procedure.md)

---

### SD-WAN Failover Procedure

Defines failover and recovery testing for the primary and secondary internet
service provider connections.

The procedure includes:

- Physical WAN1 failure
- Upstream-path failure while the interface remains active
- Performance SLA validation
- Single SLA-target failure
- Complete WAN failure
- Failback validation
- Routing and session verification
- Rollback procedures

[View SD-WAN Failover Procedure](sdwan-failover-procedure.md)

---

### Configuration Rollback Plan

Defines controlled rollback procedures for configuration changes that cause
unexpected service or security impact.

The rollback plan covers:

- Firewall-policy rollback
- Address and service object rollback
- Security-profile rollback
- SD-WAN and routing rollback
- Virtual IP rollback
- Management-access recovery
- High-Availability recovery
- Full configuration restoration
- Post-rollback validation

[View Configuration Rollback Plan](rollback-plan.md)

---

### Troubleshooting Guide

Provides a structured troubleshooting workflow for the FortiGate Enterprise
Security Lab.

The guide covers:

- System health checks
- Interfaces and VLANs
- Routing and SD-WAN
- Firewall-policy matching
- Session inspection
- Debug flow
- Packet capture
- NAT and Virtual IP
- DNS and NTP
- Security profiles
- Centralized logging
- High Availability
- Troubleshooting evidence collection

[View Troubleshooting Guide](troubleshooting-guide.md)

---

### Validation Test Results

Records the actual results of the test cases defined in the Validation Test
Plan.

The results document records:

- Test identifier
- Test scenario
- Expected result
- Actual result
- Matched firewall policy
- Logging result
- Test status
- Supporting evidence

Tests remain marked as `NOT TESTED` until they are executed in a controlled
laboratory environment.

[View Validation Test Results](test-results.md)

---

### Implementation Evidence Index

Maps security controls and validation test cases to sanitized implementation
evidence stored in the `screenshots` directory.

The evidence index includes:

- Evidence identifiers
- Related security controls
- Related test cases
- Planned filenames
- Evidence status
- Review requirements
- Screenshot sanitization checklist

[View Implementation Evidence Index](implementation-evidence-index.md)

---

## Document Status

| Document | Status |
|---|---|
| Firewall Policy Matrix | Complete |
| Implementation Plan | Complete |
| Security Hardening Checklist | Complete |
| Validation Test Plan | Complete |
| Security Risk Register | Complete |
| HA Failover Procedure | Complete |
| SD-WAN Failover Procedure | Complete |
| Configuration Rollback Plan | Complete |
| Troubleshooting Guide | Complete |
| Validation Test Results | Pending Lab Execution |
| Implementation Evidence Index | Complete |
| Sanitized Implementation Evidence | Pending Lab Execution |

> Documents marked as `Complete` have been prepared and reviewed as design or
> procedural documentation. This status does not mean that all documented
> controls have been executed or technically validated.

---

## Project Validation Status

| Validation Area | Status |
|---|---|
| Architecture Review | Complete |
| Network Addressing Review | Complete |
| Firewall Policy Design Review | Complete |
| Configuration Sanitization Review | Complete |
| FortiOS CLI Compatibility Testing | Pending Lab Execution |
| Firewall Policy Testing | Pending Lab Execution |
| Security Profile Testing | Pending Lab Execution |
| SD-WAN Failover Testing | Pending Lab Execution |
| High-Availability Failover Testing | Pending Lab Execution |
| Centralized Logging Validation | Pending Lab Execution |
| Implementation Evidence Review | Pending Lab Execution |

---

## Related Project Resources

- [Main Project README](../README.md)
- [Sanitized FortiGate Configurations](../configs/README.md)
- [Architecturene isolation
- Controlled implementation
- Documented validation
- Configuration sanitization
- Evidence-based testing
- Risk-based security engineering
- Clear separation between designed and tested controls

A control is not marked as successfully tested until it has been executed and
validated in a controlled laboratory environment.

All test results must include:

- Execution date
- Responsible tester
- Expected result
- Actual result
- Final status
- Sanitized supporting evidence

---

## Documentation Maintenance

The documentation should be reviewed when:

- A firewall policy is added or modified
- A new VLAN or security zone is introduced
- An internet-facing application is published
- The FortiOS version changes
- Security profiles are updated
- The SD-WAN design changes
- High-Availability settings change
- The logging architecture changes
- A validation test identifies unexpected behavior
- A new project release is created

Document changes should use clear commit messages and must not introduce
credentials, production information, or confidential identifiers.

---

## Current Project Phase

The project design and documentation phases are complete.

The following activities depend on controlled lab execution:

1. Import and validate the sanitized FortiOS configurations
2. Verify interface and VLAN connectivity
3. Validate firewall-policy matching
4. Validate source NAT and destination NAT
5. Test security-profile enforcement
6. Test SD-WAN failover and recovery
7. Test FortiGate High Availability
8. Validate centralized logging
9. Record actual test results
10. Upload sanitized evidence
11. Complete the final project review
12. Publish the first project release

---

## Disclaimer

All organizations, systems, IP addresses, domain names, usernames, and
configurations in this directory are fictional or sanitized.

No production configuration, credentials, private keys, certificates,
customer information, employer information, or confidential data is included.

Configuration examples and operational procedures must be independently
reviewed, adapted, approved, and tested before use in any real environment.

The project does not represent an official Fortinet deployment guide or a
production-ready configuration.
