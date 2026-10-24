# FortiGate Enterprise Security Lab  
## Implementation Plan

This document defines the implementation approach for the fictional FortiGate
Enterprise Security Lab.

The implementation covers dual internet connectivity, FortiGate
Active-Passive High Availability, SD-WAN, VLAN segmentation, firewall
policies, security profiles, DMZ application publishing, restricted
management access, and centralized logging.

> **Disclaimer**
>
> This implementation plan was created exclusively for educational and
> portfolio purposes.
>
> All company names, hostnames, domain names, IP addresses, configurations,
> and systems in this document are fictional or sanitized.
>
> This plan must not be used directly in a production environment without
> technical review, security assessment, compatibility validation, change
> approval, backup, controlled testing, and rollback planning.

---

# 1. Document Information

| Item | Value |
|---|---|
| Project | FortiGate Enterprise Security Lab |
| Organization | Example Manufacturing Ltd. |
| Environment | Fictional Enterprise Laboratory |
| Document Type | Network Security Implementation Plan |
| Firewall Platform | FortiGate Next-Generation Firewall |
| Firmware Baseline | FortiOS 7.6.x |
| Deployment Model | Active-Passive High Availability |
| Internet Design | Dual ISP with SD-WAN |
| Inspection Mode | Flow-based |
| Change Classification | Planned Lab Implementation |
| Risk Level | Medium |
| Author | Elfan Mahrobi |
| Status | Ready for Lab Implementation |
| Classification | Public / Portfolio |

---

# 2. Purpose

The purpose of this implementation plan is to provide a controlled and
repeatable process for deploying the FortiGate Enterprise Security Lab.

This document describes:

- Implementation scope
- Network and security assumptions
- Technical dependencies
- Pre-implementation checks
- Configuration sequence
- Validation procedures
- Security controls
- Expected results
- Rollback procedures
- Implementation evidence
- Project completion criteria

---

# 3. Project Objectives

The implementation aims to:

- Deploy a resilient FortiGate firewall architecture
- Configure Active-Passive High Availability
- Implement dual ISP connectivity
- Configure SD-WAN health monitoring and failover
- Segment the network using VLANs
- Separate users, servers, guests, management, DMZ, and security systems
- Apply least-privilege firewall policies
- Protect outbound traffic using security profiles
- Publish a fictional HTTPS application through a Virtual IP
- Restrict DMZ access to internal resources
- Isolate guest traffic from enterprise networks
- Restrict firewall management to an approved workstation
- Forward security and system events to a central logging server
- Validate normal traffic, denied traffic, failover, and security inspection
- Produce sanitized implementation evidence for the portfolio

---

# 4. Scope

## 4.1 In Scope

The following implementation activities are included:

- FortiGate system baseline
- WAN interface configuration
- VLAN interface configuration
- IP addressing
- Address objects and address groups
- Service objects and service groups
- SD-WAN members
- SD-WAN health checks
- Default routing
- Firewall policies
- Explicit deny controls
- Source NAT
- Destination NAT
- Virtual IP configuration
- Antivirus profile
- Intrusion Prevention System profiles
- Web-filter profiles
- DNS-filter profiles
- Application-control profiles
- SSL certificate inspection
- Management access restriction
- Local-in policy
- Administrator trusted host restriction
- Centralized Syslog configuration
- Active-Passive High Availability
- HA heartbeat and synchronization
- Functional connectivity testing
- Firewall policy testing
- Security-profile testing
- SD-WAN failover testing
- HA failover testing
- Configuration backup
- Documentation and sanitized evidence

## 4.2 Out of Scope

The following capabilities are not included in the current implementation:

- Production deployment
- FortiManager centralized management
- FortiAnalyzer reporting
- Microsoft Entra ID integration
- SAML authentication
- FortiClient EMS integration
- Zero Trust Network Access
- Remote-access VPN
- Site-to-site IPsec VPN
- Full Active Directory integration
- Fortinet Single Sign-On
- Web Application Firewall
- Network Access Control
- Privileged Access Management
- Full deep SSL inspection
- Production penetration testing
- Infrastructure-as-Code deployment
- Production-scale performance testing

These capabilities may be added in future project versions.

---

# 5. Architecture Summary

The lab uses two FortiGate firewalls configured as an Active-Passive HA
cluster.

The firewall cluster connects to:

- Primary internet service provider
- Secondary internet service provider
- Core network switch
- Dedicated HA heartbeat interfaces
- Corporate user network
- Internal server network
- Guest network
- Management network
- DMZ network
- Security and monitoring network

The logical traffic flow is:

```text
Internet
   |
   +-- ISP-1 Primary
   |
   +-- ISP-2 Secondary
           |
           v
FortiGate Active-Passive HA Cluster
           |
           v
       Core Switch
           |
           +-- VLAN 10: Corporate Users
           +-- VLAN 20: Internal Servers
           +-- VLAN 30: Guest Network
           +-- VLAN 40: Management Network
           +-- VLAN 50: DMZ
           +-- VLAN 60: Security and Monitoring
```

The complete architecture diagram is available at:

```text
diagrams/fortigate-enterprise-network-architecture.png
```

---

# 6. Network Addressing Plan

## 6.1 WAN Networks

| Interface | FortiGate Address | Upstream Gateway | Purpose |
|---|---|---|---|
| `wan1` | `203.0.113.2/30` | `203.0.113.1` | Primary ISP |
| `wan2` | `198.51.100.2/30` | `198.51.100.1` | Secondary ISP |

## 6.2 Internal Networks

| Interface | VLAN | Subnet | Gateway | Purpose |
|---|---:|---|---|---|
| `VLAN10-USERS` | 10 | `10.10.10.0/24` | `10.10.10.1` | Corporate workstations |
| `VLAN20-SERVERS` | 20 | `10.10.20.0/24` | `10.10.20.1` | Internal servers |
| `VLAN30-GUEST` | 30 | `10.10.30.0/24` | `10.10.30.1` | Guest internet access |
| `VLAN40-MGMT` | 40 | `10.10.40.0/24` | `10.10.40.1` | Administrative systems |
| `VLAN50-DMZ` | 50 | `10.10.50.0/24` | `10.10.50.1` | Public-facing applications |
| `VLAN60-SECURITY` | 60 | `10.10.60.0/24` | `10.10.60.1` | Logging and monitoring |

## 6.3 High-Availability Network

| Function | Network | Purpose |
|---|---|---|
| HA heartbeat and synchronization | `169.254.100.0/30` | HA cluster communication |

The HA heartbeat uses dedicated physical interfaces rather than ordinary
production VLAN traffic.

## 6.4 Public Service Address

| Object | Address | Purpose |
|---|---|---|
| Public Portal VIP | `203.0.113.10` | Public HTTPS application |

The public address `203.0.113.10` represents an additional
documentation-only public address routed by the fictional ISP toward
FortiGate WAN1.

The public VIP is not part of the WAN1 transit `/30` subnet.

---

# 7. Lab Systems

| Hostname | IP Address | Zone | Function |
|---|---|---|---|
| `dc01.example.local` | `10.10.20.10` | Internal Servers | Active Directory and DNS |
| `app01.example.local` | `10.10.20.20` | Internal Servers | Internal HTTPS application |
| `db01.example.local` | `10.10.20.30` | Internal Servers | PostgreSQL database |
| `ntp01.example.local` | `10.10.20.40` | Internal Servers | Internal NTP server |
| `web01.example.local` | `10.10.50.10` | DMZ | Public HTTPS application |
| `admin01.example.local` | `10.10.40.10` | Management | Administrative workstation |
| `log01.example.local` | `10.10.60.10` | Security | Central logging server |
| `monitor01.example.local` | `10.10.60.20` | Security | Monitoring server |

---

# 8. Assumptions

The implementation is based on the following assumptions:

- Two compatible FortiGate devices are available
- Both FortiGate devices run the same FortiOS version and build
- Required features are supported by the selected FortiGate model
- FortiGuard licensing is available for the required security profiles
- `wan1` and `wan2` are available as WAN interfaces
- `port3` is available as the VLAN trunk to the core switch
- `port5` and `port6` are available as HA heartbeat interfaces
- The core switch supports IEEE 802.1Q VLAN tagging
- VLANs 10, 20, 30, 40, 50, and 60 are configured on the core switch
- The ISP gateways are reachable from the FortiGate WAN interfaces
- The fictional ISP routes `203.0.113.10/32` toward WAN1
- The central log server accepts the configured Syslog transport
- Administrative access is available through console during implementation
- No production data is stored in the lab
- Test endpoints are available in each required VLAN
- A validated backup can be restored if rollback is required
- Configuration is performed during an approved lab maintenance window

---

# 9. Dependencies

## 9.1 Hardware Dependencies

- Two compatible FortiGate physical or virtual appliances
- Core switch with VLAN trunk support
- Test endpoint for Corporate Users VLAN
- Test endpoint for Guest VLAN
- Administrative workstation
- Internal server test systems
- DMZ web server
- Central log server
- Monitoring server
- Console or out-of-band management access

## 9.2 Software Dependencies

- Compatible FortiOS 7.6.x release
- Web browser for FortiGate GUI access
- SSH client
- Packet capture utility
- Syslog service
- HTTPS web server
- PostgreSQL service or simulated TCP/5432 service
- DNS test service
- NTP test service

## 9.3 Licensing Dependencies

The following capabilities may depend on active FortiGuard subscriptions:

- Antivirus
- Intrusion Prevention System
- Web filtering
- DNS filtering
- Application control
- Security rating and threat-intelligence updates

If FortiGuard licensing is unavailable, the affected tests must be marked as:

```text
NOT TESTED - LICENSE DEPENDENCY
```

## 9.4 Documentation Dependencies

The following repository files should be reviewed before implementation:

```text
README.md
documentation/firewall-policy-matrix.md
documentation/implementation-plan.md
configs/README.md
configs/01-system-settings.conf
configs/02-interfaces-vlans.conf
configs/03-address-objects.conf
configs/04-service-objects.conf
configs/05-security-profiles.conf
configs/06-sdwan.conf
configs/07-routing.conf
configs/08-vip.conf
configs/09-firewall-policies.conf
configs/10-local-in-management.conf
configs/11-logging.conf
configs/12-ha-primary.conf
configs/13-ha-secondary.conf
configs/14-validation-commands.txt
```

---

# 10. Roles and Responsibilities

| Role | Responsibility |
|---|---|
| Security Engineer | Configure FortiGate security controls and policies |
| Network Engineer | Configure switching, VLANs, trunks, routing, and ISP connectivity |
| System Administrator | Prepare servers, DNS, NTP, logging, and test services |
| Application Owner | Validate internal and public application functionality |
| Test Engineer | Execute and document test cases |
| Change Approver | Review implementation and rollback plans |
| Project Author | Maintain sanitized portfolio documentation |

For this fictional lab, one person may perform multiple roles.

---

# 11. Risk Summary

The primary implementation risks are:

- Loss of administrative access
- Incorrect WAN addressing
- Incorrect VLAN tagging
- Firewall policy ordering error
- Excessive firewall access
- Missing security-profile dependency
- SD-WAN failover not operating as expected
- HA synchronization failure
- HA split-brain condition
- Public VIP not reachable
- DMZ server communicating too broadly with internal servers
- Syslog forwarding failure
- Application disruption
- Configuration incompatibility with the FortiOS build
- Sensitive information accidentally included in evidence

Risk controls include:

- Console access
- Configuration backup
- Staged implementation
- Validation after each stage
- Least-privilege firewall policies
- Explicit rollback checkpoints
- HA status verification
- Negative traffic testing
- Configuration sanitization review

Detailed risks should be recorded in:

```text
documentation/risk-register.md
```

---

# 12. Pre-Implementation Checklist

## 12.1 FortiGate Readiness

- [ ] FortiGate model has been identified
- [ ] FortiOS version and build have been recorded
- [ ] Both HA devices use identical FortiOS versions
- [ ] Device compatibility has been confirmed
- [ ] Console access is working
- [ ] Administrative credentials are available
- [ ] Existing configuration has been backed up
- [ ] Factory-default recovery method is known
- [ ] Required ports are physically available
- [ ] FortiGuard connectivity has been verified
- [ ] Required licenses have been verified
- [ ] System time and timezone have been reviewed

## 12.2 Network Readiness

- [ ] WAN1 test connectivity is available
- [ ] WAN2 test connectivity is available
- [ ] WAN gateway addresses have been verified
- [ ] Core-switch trunk port has been prepared
- [ ] VLAN 10 has been created
- [ ] VLAN 20 has been created
- [ ] VLAN 30 has been created
- [ ] VLAN 40 has been created
- [ ] VLAN 50 has been created
- [ ] VLAN 60 has been created
- [ ] VLAN tagging has been verified
- [ ] No subnet overlaps exist
- [ ] Test endpoints are assigned to the correct VLANs
- [ ] Default gateways are correctly configured
- [ ] Routing for the public VIP has been prepared

## 12.3 Server Readiness

- [ ] `dc01` is reachable in VLAN 20
- [ ] `app01` is reachable in VLAN 20
- [ ] `db01` is listening on the expected test service
- [ ] `ntp01` provides time synchronization
- [ ] `web01` provides HTTPS service
- [ ] `admin01` is available in VLAN 40
- [ ] `log01` accepts the selected Syslog transport
- [ ] `monitor01` is available in VLAN 60

## 12.4 Security Readiness

- [ ] Security-profile names have been reviewed
- [ ] FortiGuard category IDs have been verified
- [ ] SSL inspection approach has been approved
- [ ] Management access requirements have been confirmed
- [ ] Firewall policy matrix has been reviewed
- [ ] Test cases have been prepared
- [ ] No production credentials are stored in configuration files
- [ ] No employer or customer data is included

## 12.5 Change and Recovery Readiness

- [ ] Implementation window has been defined
- [ ] Implementation owner has been assigned
- [ ] Test owner has been assigned
- [ ] Rollback decision authority has been defined
- [ ] Configuration backup has been stored securely
- [ ] Rollback procedure has been reviewed
- [ ] Success criteria have been agreed
- [ ] Evidence collection location has been prepared

---

# 13. Implementation Strategy

The implementation uses a phased approach.

Each phase must be:

1. Implemented
2. Reviewed
3. Validated
4. Documented
5. Approved before continuing

The implementation should not proceed to the next phase if a critical
validation check fails.

---

# 14. Implementation Sequence

## Phase 1: Backup and Baseline Validation

### Objective

Confirm the current device state and create a recovery point.

### Activities

1. Connect to each FortiGate using console access.
2. Record the model and FortiOS build.
3. Record the current interface mapping.
4. Export the current configuration.
5. Verify that the backup can be accessed.
6. Review available physical interfaces.
7. Confirm licensing and FortiGuard connectivity.
8. Confirm that no production data is present.

### Suggested Commands

```shell
get system status
get system performance status
show system interface
get system interface physical
```

### Validation

- [ ] Device status is healthy
- [ ] Required interfaces are available
- [ ] FortiOS version has been documented
- [ ] Configuration backup has completed
- [ ] Console access remains available

### Rollback Checkpoint

If the system baseline is unexpected, stop implementation and restore or
rebuild the lab before proceeding.

---

## Phase 2: Configure System Baseline

### Objective

Apply basic system settings required for the lab.

### Configuration File

```text
configs/01-system-settings.conf
```

### Activities

1. Configure the FortiGate hostname.
2. Configure the administrative session timeout.
3. Restrict administrative HTTPS versions.
4. Configure DNS servers.
5. Configure NTP synchronization.
6. Verify system time.

### Validation

```shell
get system status
show system global
show system dns
show system ntp
```

### Expected Result

- Hostname is correct
- Administrative timeout is configured
- DNS settings are present
- NTP server is configured
- System time is correct

### Rollback

Remove or restore the previous system settings from backup.

---

## Phase 3: Configure WAN and VLAN Interfaces

### Objective

Configure external connectivity and internal network segmentation.

### Configuration File

```text
configs/02-interfaces-vlans.conf
```

### Activities

1. Configure WAN1.
2. Configure WAN2.
3. Configure `port3` as the VLAN parent interface.
4. Configure VLAN 10 for corporate users.
5. Configure VLAN 20 for internal servers.
6. Configure VLAN 30 for guests.
7. Configure VLAN 40 for management.
8. Configure VLAN 50 for the DMZ.
9. Configure VLAN 60 for security systems.
10. Restrict administrative access to VLAN 40.
11. Verify physical and logical interface states.

### Validation

```shell
show system interface
get system interface physical
execute ping 203.0.113.1
execute ping 198.51.100.1
```

### Endpoint Validation

Test one endpoint in each VLAN:

```text
VLAN 10 Gateway: 10.10.10.1
VLAN 20 Gateway: 10.10.20.1
VLAN 30 Gateway: 10.10.30.1
VLAN 40 Gateway: 10.10.40.1
VLAN 50 Gateway: 10.10.50.1
VLAN 60 Gateway: 10.10.60.1
```

### Expected Result

- WAN gateways are reachable
- VLAN interfaces are up
- Test endpoints reach their assigned gateway
- Management services are available only on VLAN 40
- No unintended inter-VLAN communication is allowed

### Rollback

Disable or remove the newly created VLAN interfaces and restore the original
interface configuration.

---

## Phase 4: Configure Address Objects

### Objective

Create reusable and descriptive firewall address objects.

### Configuration File

```text
configs/03-address-objects.conf
```

### Activities

1. Create subnet objects.
2. Create host objects.
3. Create RFC1918 objects.
4. Create address groups.
5. Verify object names and addresses.
6. Confirm that no object contains production information.

### Validation

```shell
show firewall address
show firewall addrgrp
```

### Expected Result

All subnets, systems, and groups match the approved architecture and policy
matrix.

### Rollback

Delete incorrect objects only if they are not referenced by another
configuration.

---

## Phase 5: Configure Service Objects

### Objective

Create approved service objects and groups.

### Configuration File

```text
configs/04-service-objects.conf
```

### Activities

1. Create DNS service.
2. Create Kerberos service.
3. Create LDAP and LDAPS services.
4. Create NTP service.
5. Create PostgreSQL service.
6. Create Syslog over TLS service.
7. Create SNMP service.
8. Create service groups.
9. Verify protocol and port definitions.

### Validation

```shell
show firewall service custom
show firewall service group
```

### Expected Result

Each service object contains only the required protocol and destination port.

### Rollback

Remove incorrect service objects after confirming that the objects are not
referenced.

---

## Phase 6: Configure Security Profiles

### Objective

Create security-inspection profiles for corporate, guest, internal, and
internet-facing traffic.

### Configuration File

```text
configs/05-security-profiles.conf
```

### Profiles

```text
AV-CORPORATE
IPS-CORPORATE
IPS-PROTECT-SERVER
WEB-CORPORATE
WEB-GUEST
DNS-CORPORATE
DNS-GUEST
APP-CORPORATE
APP-GUEST
```

### Activities

1. Verify FortiGuard connectivity.
2. Review available IPS signatures.
3. Verify application category identifiers.
4. Configure antivirus inspection.
5. Configure corporate IPS.
6. Configure server-protection IPS.
7. Configure corporate web filtering.
8. Configure guest web filtering.
9. Configure corporate DNS filtering.
10. Configure guest DNS filtering.
11. Configure application control.
12. Confirm certificate-inspection availability.
13. Review profile logs.

### Validation

```shell
show antivirus profile
show ips sensor
show webfilter profile
show dnsfilter profile
show application list
show firewall ssl-ssh-profile
```

### Expected Result

- Required profiles exist
- Profile names match firewall policy references
- Critical and high-risk threats are blocked
- Logging is enabled
- No unsupported configuration options are present

### Rollback

Detach the affected profile from firewall policies before removing or
modifying the profile.

---

## Phase 7: Configure SD-WAN

### Objective

Configure WAN1 as the preferred link and WAN2 as the backup link.

### Configuration File

```text
configs/06-sdwan.conf
```

### Activities

1. Enable SD-WAN.
2. Add WAN1 as SD-WAN member 1.
3. Add WAN2 as SD-WAN member 2.
4. Configure the internet SLA.
5. Configure health-check targets.
6. Configure WAN1 as the preferred member.
7. Configure WAN2 as the secondary member.
8. Review SLA status.

### Validation

```shell
show system sdwan
diagnose sys sdwan health-check
diagnose sys sdwan service
execute ping 1.1.1.1
execute ping 8.8.8.8
```

### Expected Result

- WAN1 is selected under normal conditions
- WAN2 remains available
- Health-check targets are reachable
- Failed members are removed from eligible forwarding
- Recovered members return according to the configured policy

### Rollback

Disable the SD-WAN configuration and restore the previous route design.

---

## Phase 8: Configure Default Routing

### Objective

Route internet traffic through the SD-WAN zone.

### Configuration File

```text
configs/07-routing.conf
```

### Activities

1. Create the default route.
2. Associate the route with the SD-WAN zone.
3. Verify connected routes.
4. Verify the active default route.
5. Confirm no conflicting default routes exist.

### Validation

```shell
get router info routing-table all
get router info routing-table details
diagnose ip route list
execute traceroute 1.1.1.1
```

### Expected Result

Internet-bound traffic uses the active SD-WAN member.

### Rollback

Remove the new default route and restore the previous routing configuration.

---

## Phase 9: Configure the Public Virtual IP

### Objective

Publish the fictional DMZ HTTPS application.

### Configuration File

```text
configs/08-vip.conf
```

### Translation

```text
External IP   : 203.0.113.10
External Port : TCP/443
Mapped IP     : 10.10.50.10
Mapped Port   : TCP/443
```

### Activities

1. Confirm `web01` is reachable from FortiGate.
2. Confirm HTTPS is listening on `web01`.
3. Confirm the ISP route for the public VIP.
4. Create the Virtual IP.
5. Verify the VIP configuration.
6. Do not expose SSH or RDP.

### Validation

```shell
show firewall vip
execute ping 10.10.50.10
```

Packet capture example:

```shell
diagnose sniffer packet any 'host 10.10.50.10 and port 443' 4 0 l
```

### Expected Result

The VIP translates public HTTPS traffic to `web01`.

### Rollback

Disable the inbound firewall policy first, then remove the VIP if required.

---

## Phase 10: Configure Firewall Policies

### Objective

Implement approved communication paths using least-privilege access.

### Configuration File

```text
configs/09-firewall-policies.conf
```

### Policy Groups

#### Corporate Access

- Corporate users to internet
- Corporate users to internal application
- Corporate users to DNS
- Corporate users to identity services
- Corporate users to NTP
- Corporate users denied direct database access

#### Guest Access

- Guest to RFC1918 denied
- Guest to internet allowed with restrictions

#### DMZ Access

- Internet to DMZ portal
- DMZ web server to database
- DMZ web server to DNS
- DMZ web server to NTP
- Remaining DMZ-to-internal traffic denied

#### Security Operations

- Approved devices to central log server
- Monitoring server to approved systems

#### Management Access

- Administrative workstation to approved servers

### Activities

1. Review each policy before applying it.
2. Confirm referenced objects exist.
3. Confirm security profiles exist.
4. Apply policies in the approved order.
5. Verify NAT settings.
6. Verify security inspection.
7. Verify logging.
8. Review for overly broad sources, destinations, or services.
9. Confirm the implicit deny remains effective.

### Validation

```shell
show firewall policy
diagnose firewall iprope show 100004
```

### Functional Validation

- Corporate HTTPS internet access succeeds
- Corporate internal application access succeeds
- Corporate database access fails
- Guest internet access succeeds
- Guest access to private networks fails
- Public HTTPS access to the DMZ succeeds
- Internet SSH access fails
- DMZ database access is limited to TCP/5432
- Unauthorized DMZ access fails
- Logs are generated

### Rollback

Disable newly created policies in reverse order.

Do not delete logs or evidence before the issue has been investigated.

---

## Phase 11: Configure Restricted Management Access

### Objective

Allow FortiGate administration only from the designated workstation.

### Configuration File

```text
configs/10-local-in-management.conf
```

### Approved Management Source

```text
Hostname : admin01.example.local
Address  : 10.10.40.10
Network  : VLAN40-MGMT
```

### Approved Services

```text
HTTPS
SSH
PING
```

### Activities

1. Verify console access.
2. Confirm HTTPS and SSH are enabled only on VLAN40-MGMT.
3. Configure the local-in allow policy.
4. Configure the local-in deny policy.
5. Create or update the named administrator.
6. Configure the trusted host.
7. Configure a strong password locally.
8. Do not store the password in GitHub.
9. Test access from `admin01`.
10. Test denied access from other networks.

### Validation

```shell
show firewall local-in-policy
show system admin
```

### Expected Result

- `admin01` can manage FortiGate
- Corporate users cannot manage FortiGate
- Guest users cannot manage FortiGate
- DMZ servers cannot manage FortiGate
- WAN-side management access is unavailable

### Rollback

Use console access to disable or correct the local-in policy and administrator
trusted-host configuration.

---

## Phase 12: Configure Centralized Logging

### Objective

Forward FortiGate events to the central log server.

### Configuration File

```text
configs/11-logging.conf
```

### Log Destination

```text
Server   : log01.example.local
Address  : 10.10.60.10
Protocol : Syslog
Transport: Reliable mode
Port     : TCP/6514
Source IP: 10.10.60.1
```

### Activities

1. Confirm `log01` is reachable.
2. Confirm the selected port is listening.
3. Configure the Syslog destination.
4. Configure system event logging.
5. Configure user event logging.
6. Configure routing event logging.
7. Configure VPN event logging.
8. Configure endpoint event logging.
9. Configure HA event logging.
10. Generate a test event.
11. Confirm the event reaches `log01`.

### Validation

```shell
show log syslogd setting
show log eventfilter
execute ping 10.10.60.10
```

Packet capture example:

```shell
diagnose sniffer packet any 'host 10.10.60.10 and port 6514' 4 0 l
```

### Expected Result

Traffic, system, security, and administrative events are received by the
central log server.

### Rollback

Disable remote Syslog forwarding and restore the previous logging
configuration.

---

## Phase 13: Configure High Availability

### Objective

Form an Active-Passive FortiGate HA cluster.

### Configuration Files

Primary:

```text
configs/12-ha-primary.conf
```

Secondary:

```text
configs/13-ha-secondary.conf
```

### HA Parameters

```text
Cluster name       : FGT-ENTERPRISE-HA
Cluster ID         : 10
Mode               : Active-Passive
Primary priority   : 200
Secondary priority : 100
Heartbeat links    : port5 and port6
Session pickup     : Enabled
Monitored links    : wan1, wan2, and port3
```

### Activities

1. Confirm the devices use identical FortiOS builds.
2. Confirm heartbeat cabling.
3. Confirm no production traffic is using the heartbeat ports.
4. Configure the secondary device first where appropriate.
5. Configure the primary device.
6. Enter the secure HA password locally.
7. Do not commit the HA password to GitHub.
8. Verify that the devices form one cluster.
9. Confirm primary and secondary roles.
10. Confirm configuration synchronization.
11. Confirm monitored interfaces.
12. Confirm session pickup.
13. Back up the cluster configuration.

### Validation

```shell
get system ha status
diagnose sys ha status
```

### Expected Result

- Both devices appear in the cluster
- FGT-EDGE-01 is the preferred primary
- FGT-EDGE-02 is the secondary
- Configuration status is synchronized
- Heartbeat links are operational
- No split-brain condition exists
- Monitored interfaces are healthy

### Rollback

If the HA cluster does not form correctly:

1. Disconnect the heartbeat links if necessary.
2. Access each device through console.
3. Disable HA on the affected unit.
4. Restore the standalone configuration.
5. Confirm the primary firewall can forward traffic independently.
6. Review the firmware, group, password, heartbeat, and interface settings.

---

# 15. Validation Plan

Validation must cover:

- Configuration presence
- Allowed traffic
- Denied traffic
- NAT
- Security inspection
- Management access
- Logging
- SD-WAN failover
- HA failover
- Recovery behavior

The detailed test plan should be maintained in:

```text
documentation/test-plan.md
```

---

# 16. Core Functional Test Cases

## TC-001: Corporate Internet Access

```text
Source          : Corporate user
Destination     : Approved HTTPS website
Expected Result : Allowed
NAT             : Applied
Security        : Inspected
Logging         : Enabled
```

## TC-002: Corporate Internal Application Access

```text
Source          : Corporate user
Destination     : 10.10.20.20:443
Expected Result : Allowed
NAT             : Disabled
Logging         : Enabled
```

## TC-003: Corporate Direct Database Access

```text
Source          : Corporate user
Destination     : 10.10.20.30:5432
Expected Result : Blocked
Logging         : Enabled
```

## TC-004: Guest Internet Access

```text
Source          : Guest client
Destination     : Approved internet website
Expected Result : Allowed
NAT             : Applied
Security        : Restricted guest profiles
```

## TC-005: Guest Access to Internal Server

```text
Source          : Guest client
Destination     : 10.10.20.20:443
Expected Result : Blocked
Logging         : Enabled
```

## TC-006: Internet Access to Public Portal

```text
Source          : Internet test client
Destination     : 203.0.113.10:443
Mapped Server   : 10.10.50.10:443
Expected Result : Allowed through VIP
Logging         : Enabled
```

## TC-007: Internet Administrative Access

```text
Source          : Internet test client
Destination     : 203.0.113.10:22
Expected Result : Blocked
Logging         : Enabled
```

## TC-008: DMZ Web Server to Database

```text
Source          : 10.10.50.10
Destination     : 10.10.20.30:5432
Expected Result : Allowed
Logging         : Enabled
```

## TC-009: Unauthorized DMZ Access

```text
Source          : 10.10.50.10
Destination     : Unauthorized internal service
Expected Result : Blocked
Logging         : Enabled
```

## TC-010: FortiGate Management Access

```text
Source          : 10.10.40.10
Destination     : 10.10.40.1:443
Expected Result : Allowed
```

## TC-011: Unauthorized Management Access

```text
Source          : Corporate, Guest, Server, or DMZ network
Destination     : FortiGate administrative service
Expected Result : Blocked
Logging         : Enabled
```

## TC-012: Syslog Forwarding

```text
Source          : FortiGate
Destination     : 10.10.60.10:6514
Expected Result : Log received
```

## TC-013: SD-WAN Failover

```text
Action          : Simulate WAN1 failure
Expected Result : WAN2 becomes active
User Impact     : Limited to session reconvergence
Logging         : WAN and SLA event recorded
```

## TC-014: SD-WAN Recovery

```text
Action          : Restore WAN1
Expected Result : WAN1 returns according to priority and SLA policy
Logging         : Recovery event recorded
```

## TC-015: HA Failover

```text
Action          : Simulate primary failure
Expected Result : Secondary becomes active
Configuration   : Remains synchronized
Logging         : HA event recorded
```

## TC-016: HA Recovery

```text
Action          : Restore primary device
Expected Result : Cluster returns to the intended role state
Split Brain     : Must not occur
```

---

# 17. Security Profile Validation

Security-profile testing should use safe and authorized test methods.

## Antivirus

Validate that:

- The antivirus profile is attached
- A safe antivirus test method is blocked
- The event appears in logs

## Intrusion Prevention

Validate that:

- The IPS profile is attached
- Approved test traffic triggers the expected action
- Critical and high-severity events are logged
- Legitimate application traffic is not disrupted

## Web Filtering

Validate that:

- Allowed categories remain accessible
- Blocked test categories are denied
- The block event appears in logs

## DNS Filtering

Validate that:

- Approved domains resolve
- Blocked test domains are denied
- Botnet-domain protection is enabled where licensed
- DNS-filter events appear in logs

## Application Control

Validate that:

- Approved applications remain functional
- Restricted test applications are blocked or monitored
- Unknown applications are logged

## SSL Inspection

Validate that:

- Certificate inspection is active
- Valid certificates are handled correctly
- Invalid certificate conditions generate expected logs
- Deep SSL inspection is not unintentionally enabled

---

# 18. SD-WAN Failover Procedure

## Test Steps

1. Confirm WAN1 is active.
2. Confirm WAN2 is healthy.
3. Start continuous connectivity monitoring.
4. Disconnect or disable WAN1 in the lab.
5. Observe SD-WAN health-check status.
6. Confirm traffic transitions to WAN2.
7. Record failover time.
8. Validate internet access.
9. Restore WAN1.
10. Confirm WAN1 becomes healthy.
11. Confirm path selection returns according to the configured policy.
12. Review event logs.

## Commands

```shell
diagnose sys sdwan health-check
diagnose sys sdwan service
get router info routing-table all
```

## Success Criteria

- WAN1 failure is detected
- WAN2 becomes eligible
- Internet access is restored through WAN2
- Logs record the failure
- WAN1 recovery is detected
- Routing returns to the intended state

---

# 19. HA Failover Procedure

## Test Steps

1. Confirm both HA members are synchronized.
2. Confirm FGT-EDGE-01 is primary.
3. Start continuous connectivity testing.
4. Record current sessions.
5. Simulate primary failure in the controlled lab.
6. Confirm FGT-EDGE-02 becomes active.
7. Validate corporate internet access.
8. Validate internal application access.
9. Validate DMZ publishing.
10. Validate management access.
11. Validate logging.
12. Restore FGT-EDGE-01.
13. Confirm cluster synchronization.
14. Confirm the final primary and secondary roles.
15. Review HA events.

## Commands

```shell
get system ha status
diagnose sys ha status
```

## Success Criteria

- Secondary firewall becomes active
- Critical connectivity is restored
- No split-brain condition occurs
- Configuration remains synchronized
- HA events are logged
- The cluster returns to a healthy state

---

# 20. Troubleshooting Procedure

If expected traffic fails:

1. Verify endpoint addressing.
2. Verify VLAN assignment.
3. Verify default gateway.
4. Verify source and destination interfaces.
5. Verify route selection.
6. Verify address objects.
7. Verify service objects.
8. Verify firewall policy order.
9. Verify NAT.
10. Verify security profiles.
11. Verify sessions.
12. Run packet capture.
13. Run debug flow if required.
14. Stop all debugging after evidence collection.

## Routing

```shell
get router info routing-table all
diagnose ip route list
```

## Sessions

```shell
diagnose sys session filter clear
diagnose sys session filter src <SOURCE-IP>
diagnose sys session filter dst <DESTINATION-IP>
diagnose sys session list
```

## Packet Capture

```shell
diagnose sniffer packet any 'host <IP-ADDRESS>' 4 0 l
```

## Debug Flow

```shell
diagnose debug reset
diagnose debug flow filter clear
diagnose debug flow filter saddr <SOURCE-IP>
diagnose debug flow filter daddr <DESTINATION-IP>
diagnose debug flow show function-name enable
diagnose debug flow trace start 100
diagnose debug enable
```

Stop debugging:

```shell
diagnose debug disable
diagnose debug reset
```

---

# 21. Rollback Strategy

Rollback may be initiated when:

- Administrative access is lost
- Critical connectivity is unavailable
- Inter-zone access is broader than intended
- Guest isolation fails
- DMZ isolation fails
- Public application publishing causes unexpected exposure
- SD-WAN causes unstable routing
- HA causes instability or split-brain
- Security profiles disrupt required traffic
- Centralized logging causes unexpected resource impact
- The configuration contains unsupported commands
- The lab cannot be returned to a stable state within the planned window

---

# 22. Rollback Procedure

## 22.1 Firewall Policy Rollback

1. Disable the newly created policy.
2. Confirm that unintended access has stopped.
3. Restore the previous policy state.
4. Clear affected sessions where necessary.
5. Retest critical traffic.

## 22.2 Security Profile Rollback

1. Detach the affected security profile.
2. Apply an approved temporary baseline profile if required.
3. Clear affected sessions.
4. Retest the application.
5. Review the profile configuration.

## 22.3 VIP Rollback

1. Disable the inbound firewall policy.
2. Confirm public access is unavailable.
3. Remove or disable the VIP.
4. Verify the DMZ server remains accessible internally if required.

## 22.4 SD-WAN Rollback

1. Disable affected SD-WAN rules.
2. Restore the previous static default route.
3. Verify internet access.
4. Review health-check configuration.

## 22.5 Management Rollback

1. Connect through console.
2. Disable the problematic local-in policy.
3. Correct interface administrative access.
4. Correct administrator trusted hosts.
5. Retest from `admin01`.

## 22.6 HA Rollback

1. Access each device through console.
2. Disconnect heartbeat links if the cluster is unstable.
3. Disable HA on the affected unit.
4. Restore the preferred device as a standalone firewall.
5. Restore the validated configuration.
6. Verify routing, policies, NAT, and management access.

## 22.7 Full Configuration Rollback

1. Stop implementation.
2. Preserve troubleshooting evidence.
3. Load the validated pre-change configuration backup.
4. Reboot only if required.
5. Verify system health.
6. Verify management access.
7. Verify WAN connectivity.
8. Verify critical internal traffic.
9. Document the rollback reason.

---

# 23. Backout Decision Matrix

| Condition | Action |
|---|---|
| Minor documentation mismatch | Correct documentation and continue |
| Non-critical policy issue | Disable affected policy and troubleshoot |
| Security profile false positive | Detach or tune affected profile |
| Guest isolation failure | Stop implementation and correct immediately |
| Management lockout | Use console and rollback local-in controls |
| Public exposure beyond HTTPS | Disable inbound policy immediately |
| Routing instability | Roll back SD-WAN or route changes |
| HA split-brain | Isolate secondary and restore standalone operation |
| Critical service unavailable | Roll back affected stage |
| Multiple critical failures | Restore full pre-change configuration |

---

# 24. Implementation Evidence

Evidence should be collected after each successful phase.

Recommended evidence:

```text
screenshots/01-system-dashboard.png
screenshots/02-interface-vlan-list.png
screenshots/03-sdwan-members.png
screenshots/04-sdwan-health-check.png
screenshots/05-address-objects.png
screenshots/06-firewall-policies.png
screenshots/07-security-profiles.png
screenshots/08-vip-configuration.png
screenshots/09-routing-table.png
screenshots/10-ha-status.png
screenshots/11-traffic-logs.png
screenshots/12-security-events.png
```

All evidence must be reviewed and sanitized before publication.

Evidence must not contain:

- Credentials
- Administrator passwords
- HA passwords
- Private keys
- Tokens
- Serial numbers
- FortiCare details
- Real public IP addresses
- Real usernames
- Employer information
- Customer information
- Production logs
- Certificate private information
- Device UUIDs
- MAC addresses where unnecessary

---

# 25. Success Criteria

The implementation is considered technically successful when:

- [ ] Both WAN interfaces are operational
- [ ] SD-WAN health checks are healthy
- [ ] WAN1 is preferred under normal conditions
- [ ] WAN2 carries traffic after WAN1 failure
- [ ] All VLAN gateways are reachable from approved endpoints
- [ ] Network segmentation operates as designed
- [ ] Corporate users can access approved internet services
- [ ] Corporate users can access the internal application
- [ ] Corporate users cannot directly access the database
- [ ] Guest users can access the internet
- [ ] Guest users cannot access private enterprise networks
- [ ] The public HTTPS portal is reachable through the VIP
- [ ] Internet administrative access is blocked
- [ ] The DMZ server can access only approved internal services
- [ ] Unauthorized DMZ-to-internal access is blocked
- [ ] FortiGate management is restricted to `admin01`
- [ ] Security profiles are attached and operational
- [ ] Traffic and security events are logged
- [ ] Logs reach the central logging server
- [ ] HA members are synchronized
- [ ] HA failover completes successfully
- [ ] No sensitive data exists in the repository
- [ ] Test results are documented accurately

---

# 26. Project Completion Criteria

The project is considered fully complete when:

- [ ] Architecture diagram is complete
- [ ] Addressing plan is complete
- [ ] Firewall policy matrix is complete
- [ ] Sanitized configuration files are complete
- [ ] Implementation plan is complete
- [ ] Hardening checklist is complete
- [ ] Test plan is complete
- [ ] Risk register is complete
- [ ] All configurations have been reviewed
- [ ] Compatible FortiOS lab validation has been completed
- [ ] SD-WAN failover has been tested
- [ ] HA failover has been tested
- [ ] Security-profile behavior has been tested
- [ ] Test results have been documented
- [ ] Sanitized evidence has been uploaded
- [ ] README documentation index has been updated
- [ ] Repository license has been selected
- [ ] Security policy has been added
- [ ] Repository release `v1.0.0` has been created
- [ ] Repository has been linked to the GitHub profile

---

# 27. Implementation Record

## Implementation Details

```text
Implementation Date :
Start Time          :
End Time            :
Implemented By      :
Reviewed By         :
FortiGate Model     :
FortiOS Version     :
FortiOS Build       :
Primary Device      :
Secondary Device    :
Change Reference    :
```

## Result

Select one:

```text
[ ] Successful
[ ] Successful with Minor Issues
[ ] Partially Successful
[ ] Rolled Back
[ ] Failed
[ ] Not Yet Executed
```

## Implementation Notes

```text
Add implementation notes here.
```

## Issues Identified

```text
List issues identified during implementation.
```

## Corrective Actions

```text
List corrective actions and owners.
```

## Evidence Location

```text
screenshots/
documentation/test-results.md
```

---

# 28. Approval Record

| Role | Name | Decision | Date | Notes |
|---|---|---|---|---|
| Implementer | Elfan Mahrobi | Pending | YYYY-MM-DD | Lab implementation |
| Technical Reviewer | To be assigned | Pending | YYYY-MM-DD | Configuration review |
| Security Reviewer | To be assigned | Pending | YYYY-MM-DD | Security-control review |
| Test Reviewer | To be assigned | Pending | YYYY-MM-DD | Test-result review |

For an individual portfolio lab, unassigned approvals should remain marked as
`To be assigned` rather than being represented as completed approvals.

---

# 29. Current Implementation Status

| Phase | Status |
|---|---|
| Architecture Design | Complete |
| Addressing Plan | Complete |
| Firewall Policy Matrix | Complete |
| Sanitized Configuration Preparation | Complete |
| Implementation Plan | Complete |
| Security Hardening Documentation | Planned |
| Test Plan | Planned |
| Risk Register | Planned |
| Lab Deployment | Not Yet Executed |
| Configuration Validation | Not Yet Executed |
| SD-WAN Failover Testing | Not Yet Executed |
| HA Failover Testing | Not Yet Executed |
| Security Profile Testing | Not Yet Executed |
| Evidence Collection | Not Yet Executed |
| Final Portfolio Release | Pending |

> Status must be updated only when supporting implementation or test evidence
> is available.

---

# 30. References

Related repository documents:

- `README.md`
- `documentation/firewall-policy-matrix.md`
- `documentation/hardening-checklist.md`
- `documentation/test-plan.md`
- `documentation/test-results.md`
- `documentation/risk-register.md`
- `configs/README.md`
- `diagrams/README.md`
- `screenshots/README.md`

Configuration files:

- `configs/01-system-settings.conf`
- `configs/02-interfaces-vlans.conf`
- `configs/03-address-objects.conf`
- `configs/04-service-objects.conf`
- `configs/05-security-profiles.conf`
- `configs/06-sdwan.conf`
- `configs/07-routing.conf`
- `configs/08-vip.conf`
- `configs/09-firewall-policies.conf`
- `configs/10-local-in-management.conf`
- `configs/11-logging.conf`
- `configs/12-ha-primary.conf`
- `configs/13-ha-secondary.conf`
- `configs/14-validation-commands.txt`

---

# 31. Final Disclaimer

This implementation plan represents a fictional FortiGate laboratory
environment.

The documented configurations must not be treated as production-ready
configurations without:

- Platform compatibility review
- Firmware-specific validation
- Security review
- Network review
- Application-owner approval
- Change approval
- Configuration backup
- Controlled testing
- Monitoring
- Rollback planning

The project does not contain production credentials, customer data, employer
information, or confidential configurations.
