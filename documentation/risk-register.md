# Security Risk Register

This document identifies, evaluates, and documents the primary security and
operational risks associated with the FortiGate Enterprise Security Lab.

The risk register covers the fictional enterprise architecture, including:

- FortiGate Active-Passive High Availability
- Dual ISP and SD-WAN connectivity
- Corporate user network
- Internal server network
- Guest network
- Management network
- Public-services DMZ
- Security monitoring network
- Firewall policies
- Security profiles
- Application publishing
- Centralized logging
- Administrative access

> **Disclaimer**
>
> All organizations, systems, domain names, IP addresses, configurations, and
> scenarios in this document are fictional or sanitized.
>
> This risk register was created exclusively for educational and portfolio
> purposes. It does not represent a risk assessment of any real organization.

---

## 1. Document Information

| Item | Value |
|---|---|
| Project | FortiGate Enterprise Security Lab |
| Document Type | Security Risk Register |
| Environment | Fictional enterprise network |
| Assessment Scope | Network, firewall, connectivity, administration, DMZ, and monitoring |
| Risk Method | Qualitative likelihood and impact assessment |
| Risk Owner | Security Engineering |
| Author | Elfan Mahrobi |
| Status | Initial Lab Assessment |
| Classification | Public / Portfolio |
| Review Frequency | After major design or configuration changes |

---

## 2. Objectives

The objectives of this risk register are to:

- Identify security and operational risks within the lab architecture
- Document the systems and security controls affected by each risk
- Assess likelihood and business impact
- Prioritize risks based on severity
- Define preventive, detective, and corrective controls
- Assign risk owners
- Record residual risk after mitigation
- Support controlled implementation and validation
- Demonstrate risk-based security engineering
- Prevent design and configuration risks from being overlooked

---

## 3. Scope

This risk assessment covers:

- FortiGate firewall configuration
- Firewall policy design
- VLAN and security-zone segmentation
- FortiGate High Availability
- HA heartbeat and synchronization
- Dual-WAN and SD-WAN configuration
- Internet connectivity
- Source NAT and destination NAT
- Virtual IP application publishing
- Corporate internet access
- Guest-network isolation
- DMZ protection
- Internal server protection
- Management-plane security
- Administrator accounts
- FortiGuard security profiles
- SSL inspection
- Centralized logging
- Network monitoring
- Configuration backup
- Firmware and vulnerability management
- Documentation and configuration sanitization

The following areas are outside the current scope:

- Physical building security
- Human resources risks
- Third-party contractual risks
- Application source-code security
- Database-level access controls
- Endpoint detection and response
- Full disaster-recovery planning
- Production service-level requirements
- Regulatory compliance certification

---

## 4. Risk Assessment Method

Each risk is assessed using:

- Likelihood
- Impact
- Inherent risk
- Control effectiveness
- Residual risk

### 4.1 Likelihood Scale

| Score | Rating | Description |
|---:|---|---|
| 1 | Rare | The event is unlikely to occur under normal conditions |
| 2 | Unlikely | The event may occur but is not expected regularly |
| 3 | Possible | The event could reasonably occur |
| 4 | Likely | The event is expected to occur without effective controls |
| 5 | Almost Certain | The event is expected to occur frequently or repeatedly |

### 4.2 Impact Scale

| Score | Rating | Description |
|---:|---|---|
| 1 | Insignificant | Minimal service or security impact |
| 2 | Minor | Limited operational impact with simple recovery |
| 3 | Moderate | Noticeable service disruption or security exposure |
| 4 | Major | Significant service outage, compromise, or data exposure |
| 5 | Severe | Critical compromise, prolonged outage, or widespread impact |

### 4.3 Risk Calculation

```text
Risk Score = Likelihood × Impact
```

### 4.4 Risk Rating

| Score | Rating | Required Response |
|---:|---|---|
| 1–4 | Low | Accept or monitor |
| 5–9 | Medium | Mitigate and periodically review |
| 10–16 | High | Prioritize mitigation and management review |
| 17–25 | Critical | Immediate treatment required before implementation |

---

## 5. Risk Summary

| ID | Risk | Likelihood | Impact | Inherent Risk | Residual Risk | Owner |
|---|---|---:|---:|---|---|---|
| R-001 | Overly permissive firewall policy | 4 | 5 | Critical | Medium | Security Engineering |
| R-002 | Incorrect firewall policy ordering | 3 | 4 | High | Low | Security Engineering |
| R-003 | Unauthorized management access | 3 | 5 | High | Low | Security Engineering |
| R-004 | Administrator account compromise | 3 | 5 | High | Medium | Security Engineering |
| R-005 | Public DMZ application compromise | 4 | 5 | Critical | Medium | Application and Security Teams |
| R-006 | Unauthorized DMZ-to-internal access | 3 | 5 | High | Low | Security Engineering |
| R-007 | Direct user access to database | 3 | 5 | High | Low | Security Engineering |
| R-008 | Guest network reaches enterprise systems | 3 | 4 | High | Low | Network Security |
| R-009 | FortiGate appliance failure | 3 | 5 | High | Low | Network Operations |
| R-010 | High Availability synchronization failure | 3 | 4 | High | Medium | Network Operations |
| R-011 | HA split-brain condition | 2 | 5 | High | Low | Network Operations |
| R-012 | Primary ISP outage | 4 | 4 | High | Low | Network Operations |
| R-013 | SD-WAN health-check failure | 3 | 4 | High | Medium | Network Operations |
| R-014 | Incorrect NAT or Virtual IP configuration | 3 | 4 | High | Low | Security Engineering |
| R-015 | Security profiles not applied | 3 | 4 | High | Low | Security Engineering |
| R-016 | FortiGuard service unavailability | 3 | 3 | Medium | Medium | Security Operations |
| R-017 | SSL inspection incompatibility | 4 | 3 | High | Medium | Security Engineering |
| R-018 | Central log server failure | 3 | 4 | High | Medium | Security Operations |
| R-019 | Missing or incomplete security logs | 3 | 4 | High | Medium | Security Operations |
| R-020 | Time synchronization failure | 3 | 3 | Medium | Low | Infrastructure Operations |
| R-021 | Unsupported or vulnerable FortiOS version | 3 | 5 | High | Medium | Security Engineering |
| R-022 | Configuration backup unavailable | 3 | 4 | High | Low | Network Operations |
| R-023 | Unauthorized configuration change | 3 | 5 | High | Medium | Security Engineering |
| R-024 | Sensitive configuration exposed in GitHub | 3 | 5 | High | Low | Repository Maintainer |
| R-025 | Insufficient implementation testing | 4 | 4 | High | Medium | Project Owner |
| R-026 | Monitoring server compromise | 3 | 4 | High | Medium | Security Operations |
| R-027 | DNS service disruption or manipulation | 3 | 4 | High | Medium | Infrastructure Operations |
| R-028 | Certificate expiration or misconfiguration | 3 | 4 | High | Medium | Application Owner |
| R-029 | Network segmentation bypass | 3 | 5 | High | Low | Security Engineering |
| R-030 | Resource exhaustion or denial of service | 3 | 5 | High | Medium | Network and Security Operations |

---

# 6. Detailed Risk Register

## R-001: Overly Permissive Firewall Policy

### Risk Description

An overly permissive firewall policy may allow unauthorized communication
between network zones, increasing the attack surface and enabling lateral
movement.

### Affected Components

- Corporate user network
- Internal server network
- Management network
- DMZ
- Security network
- Firewall policy rulebase

### Potential Causes

- Use of `all` source or destination objects
- Use of the `ALL` service
- Broad interface selection
- Missing policy justification
- Emergency rules not removed
- Incorrect interpretation of application requirements
- Lack of policy review

### Potential Impact

- Unauthorized server access
- Lateral movement
- Data exposure
- Security-zone bypass
- Increased malware propagation
- Unauthorized administrative access

### Assessment

```text
Likelihood     : 4 - Likely
Impact         : 5 - Severe
Inherent score : 20
Inherent risk  : Critical
```

### Existing Controls

- Specific source and destination objects
- Explicit interface pairs
- Restricted service objects
- Default-deny design
- Firewall policy matrix
- Traffic logging
- Policy comments
- Configuration review checklist

### Additional Mitigations

- Require peer review before implementation
- Add policy owners and review dates
- Create expiration dates for temporary rules
- Review unused and duplicate policies
- Validate positive and negative traffic flows
- Review hit counts periodically
- Remove obsolete firewall rules
- Use FortiManager workflow in an expanded design

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Security Engineering

---

## R-002: Incorrect Firewall Policy Ordering

### Risk Description

A broad policy placed above a specific policy may process traffic before the
intended security rule, resulting in unintended access or missing inspection.

### Potential Causes

- Incorrect rule placement
- Broad internet policy positioned above deny rules
- New policy inserted without reviewing existing rules
- Duplicate or overlapping policies
- Interface-pair misunderstanding

### Potential Impact

- Security controls bypassed
- Incorrect NAT behavior
- Guest or DMZ traffic allowed unexpectedly
- Security profiles not applied
- Incomplete logging

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Place specific policies above general policies
- Place explicit deny rules before broad allow rules
- Review policy lookup results
- Validate policy IDs and sequence
- Test actual matched rule in traffic logs
- Perform debug flow validation
- Document policy-ordering standards

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 4 - Major
Residual score: 4
Residual risk : Low
```

### Risk Owner

Security Engineering

---

## R-003: Unauthorized FortiGate Management Access

### Risk Description

An unauthorized device or network may gain access to the FortiGate management
interface.

### Affected Components

- FortiGate administrative GUI
- SSH management
- Management VLAN
- Administrator accounts
- Local-in policies

### Potential Causes

- Administrative access enabled on WAN
- Administrative access enabled on user VLANs
- Missing trusted-host restrictions
- Weak local-in policy
- Broad management-network access
- Use of HTTP or Telnet

### Potential Impact

- Firewall configuration modification
- Administrator account compromise
- Security-policy bypass
- Service disruption
- Configuration theft
- Complete network compromise

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Existing Controls

- Dedicated Management VLAN
- Management access from `HOST-ADMIN01`
- Interface administrative-access restrictions
- Administrator trusted hosts
- Local-in policy
- HTTPS and SSH only
- Administrative logging

### Additional Mitigations

- Enable multi-factor authentication
- Use named administrator accounts
- Use least-privilege administrator profiles
- Disable the default administrator where appropriate
- Integrate privileged-access management
- Use a jump server or privileged-access workstation
- Monitor failed administrator logins

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 5 - Severe
Residual score: 5
Residual risk : Low
```

### Risk Owner

Security Engineering

---

## R-004: Administrator Account Compromise

### Risk Description

An attacker may obtain valid administrator credentials and perform
unauthorized changes.

### Potential Causes

- Weak passwords
- Password reuse
- Credential phishing
- Shared administrator accounts
- Missing MFA
- Credentials stored in scripts
- Excessive administrator permissions

### Potential Impact

- Firewall takeover
- Security policy modification
- Logging disabled
- Backdoor rules created
- Configuration exported
- Complete outage

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Mitigations

- Strong unique passwords
- Multi-factor authentication
- Individual named accounts
- Trusted-host restrictions
- Least-privilege access profiles
- Administrator activity logging
- Regular account review
- Disable inactive accounts
- Do not store passwords in GitHub
- Use secure credential-management processes

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 5 - Severe
Residual score: 10
Residual risk : Medium
```

### Risk Owner

Security Engineering

---

## R-005: Public DMZ Application Compromise

### Risk Description

The internet-facing web application may be compromised through application,
web-server, operating-system, or configuration vulnerabilities.

### Affected Components

- `portal.example.com`
- Public VIP `203.0.113.10`
- `web01.example.local`
- DMZ VLAN
- Database connectivity
- Internal DNS and NTP services

### Potential Causes

- Vulnerable web application
- Missing security updates
- Weak authentication
- Exposed unnecessary services
- Web-server misconfiguration
- Missing WAF protection
- Insecure application dependencies

### Potential Impact

- Web application compromise
- Credential theft
- Malware installation
- DMZ persistence
- Attacks against internal systems
- Public-service disruption
- Data exposure

### Assessment

```text
Likelihood     : 4 - Likely
Impact         : 5 - Severe
Inherent score : 20
Inherent risk  : Critical
```

### Existing Controls

- HTTPS-only publishing
- Destination NAT through a dedicated VIP
- Server-focused IPS
- DMZ segmentation
- Restricted DMZ-to-internal access
- Full traffic logging
- No public SSH or RDP exposure

### Additional Mitigations

- Deploy a Web Application Firewall
- Perform vulnerability assessments
- Perform penetration testing
- Patch the application and operating system
- Apply secure coding standards
- Use strong authentication
- Implement application-level logging
- Monitor file integrity
- Apply endpoint protection to the server
- Use a dedicated DMZ DNS resolver
- Apply rate limiting and DDoS protection

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 5 - Severe
Residual score: 10
Residual risk : Medium
```

### Risk Owner

Application Owner and Security Engineering

---

## R-006: Unauthorized DMZ-to-Internal Access

### Risk Description

A compromised DMZ server may communicate with internal systems beyond the
services required by the application.

### Potential Impact

- Lateral movement
- Internal server compromise
- Database compromise
- Active Directory exposure
- Data exfiltration

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Existing Controls

- Specific `WEB01-to-DB01` policy
- TCP/5432-only database access
- Specific DNS and NTP policies
- Explicit DMZ-to-internal deny policy
- IPS inspection
- Traffic logging
- No source NAT for internal communication

### Additional Mitigations

- Use a dedicated database account with minimum permissions
- Encrypt database communication
- Monitor unusual database activity
- Use a dedicated DMZ DNS resolver
- Apply host-based firewall rules
- Conduct periodic policy reviews

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 5 - Severe
Residual score: 5
Residual risk : Low
```

### Risk Owner

Security Engineering

---

## R-007: Direct Corporate User Access to Database

### Risk Description

Corporate workstations may attempt to connect directly to the database server,
bypassing application-layer access controls.

### Potential Impact

- Unauthorized database access
- Credential attacks
- Data manipulation
- Sensitive information exposure
- Database denial of service

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Existing Controls

- Explicit user-to-database deny policy
- Database access limited to `HOST-WEB01`
- Segmented server VLAN
- Denied-traffic logging

### Additional Mitigations

- Enable host-based firewall controls
- Restrict database listener addresses
- Apply strong database authentication
- Encrypt database sessions
- Monitor unsuccessful database logins
- Periodically validate policy enforcement

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 5 - Severe
Residual score: 5
Residual risk : Low
```

### Risk Owner

Security Engineering and Database Owner

---

## R-008: Guest Network Access to Enterprise Systems

### Risk Description

Guest clients may access corporate, server, management, DMZ, or security
networks because of incorrect firewall policy or routing configuration.

### Potential Impact

- Unauthorized internal access
- Malware propagation
- Internal reconnaissance
- Administrative-interface exposure
- Policy and compliance violations

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Existing Controls

- Dedicated Guest VLAN
- Explicit guest-to-RFC1918 deny policy
- Internet-only access
- Source NAT
- Guest-specific web and application controls
- Traffic logging

### Additional Mitigations

- Enable wireless client isolation
- Apply bandwidth limits
- Use captive-portal controls where appropriate
- Validate access to every internal subnet
- Monitor denied guest traffic
- Separate guest DNS services

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 4 - Major
Residual score: 4
Residual risk : Low
```

### Risk Owner

Network Security

---

## R-009: FortiGate Appliance Failure

### Risk Description

The active FortiGate may experience hardware, virtual appliance, operating
system, or interface failure.

### Potential Impact

- Internet outage
- Inter-zone outage
- Public application outage
- Loss of security inspection
- Management disruption

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Existing Controls

- Active-Passive HA cluster
- Interface monitoring
- Session synchronization
- Dedicated heartbeat links
- Secondary firewall
- HA event logging

### Additional Mitigations

- Test HA failover regularly
- Use redundant power and switching
- Monitor HA health continuously
- Maintain compatible firmware versions
- Keep configuration backups
- Document replacement procedures
- Validate session pickup

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 5 - Severe
Residual score: 5
Residual risk : Low
```

### Risk Owner

Network Operations

---

## R-010: HA Synchronization Failure

### Risk Description

The secondary FortiGate may not receive current configuration or session
information from the primary firewall.

### Potential Impact

- Inconsistent policies after failover
- Session loss
- Application outage
- Incorrect routing
- Missing objects or security profiles

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Use two heartbeat interfaces
- Monitor HA synchronization status
- Keep FortiOS versions consistent
- Validate checksum and cluster status
- Test configuration synchronization
- Test controlled failover
- Investigate out-of-sync states immediately
- Protect heartbeat connections physically

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Network Operations

---

## R-011: HA Split-Brain Condition

### Risk Description

Both FortiGate devices may become active if heartbeat communication fails,
causing duplicate IP addresses, inconsistent forwarding, and service
instability.

### Potential Causes

- Both heartbeat links fail
- Incorrect heartbeat configuration
- Layer 2 connectivity issue
- Firmware defect
- Cabling or virtual networking issue

### Potential Impact

- Duplicate IP addresses
- Routing instability
- Session interruption
- Inconsistent policy behavior
- Network outage

### Assessment

```text
Likelihood     : 2 - Unlikely
Impact         : 5 - Severe
Inherent score : 10
Inherent risk  : High
```

### Mitigations

- Use two dedicated heartbeat interfaces
- Use direct or isolated heartbeat connectivity
- Monitor cluster state
- Test heartbeat-link failure
- Keep cluster members on compatible firmware
- Document split-brain recovery
- Avoid sharing heartbeat interfaces with user traffic

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 5 - Severe
Residual score: 5
Residual risk : Low
```

### Risk Owner

Network Operations

---

## R-012: Primary ISP Outage

### Risk Description

The primary internet service provider may become unavailable or experience
severe degradation.

### Potential Impact

- Internet-access interruption
- Public portal outage
- Remote service disruption
- Failed DNS or security updates
- Business-service degradation

### Assessment

```text
Likelihood     : 4 - Likely
Impact         : 4 - Major
Inherent score : 16
Inherent risk  : High
```

### Existing Controls

- Secondary ISP
- SD-WAN priority rule
- Internet SLA health checks
- Automatic route updates
- WAN monitoring

### Additional Mitigations

- Test failover and recovery
- Use diverse carrier paths where possible
- Monitor latency, loss, and jitter
- Validate inbound-service failover requirements
- Document ISP escalation procedures
- Assess whether the public VIP requires secondary-path publishing

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 4 - Major
Residual score: 4
Residual risk : Low
```

### Risk Owner

Network Operations

---

## R-013: SD-WAN Health-Check Failure

### Risk Description

An incorrect or unreliable SD-WAN health check may mark a healthy link as
failed or retain an unusable link.

### Potential Impact

- Traffic sent through failed path
- Unnecessary link failover
- Intermittent connectivity
- Increased latency
- Session interruption

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Use multiple reliable SLA targets
- Validate SLA intervals and thresholds
- Monitor loss, latency, and jitter
- Test complete upstream failure
- Test gateway-only failure
- Test recovery and failback
- Avoid using a single health-check target
- Document expected SD-WAN member priority

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Network Operations

---

## R-014: Incorrect NAT or Virtual IP Configuration

### Risk Description

Incorrect source NAT, destination NAT, interface binding, port mapping, or
public routing may prevent access or expose unintended services.

### Potential Impact

- Public application unavailable
- Wrong internal server exposed
- Unintended port exposure
- Asymmetric routing
- Missing original source information

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Restrict the VIP to TCP/443
- Bind the VIP to `wan1`
- Validate public IP routing
- Confirm mapped IP and mapped port
- Test unauthorized ports
- Review NAT using debug flow and packet capture
- Document that `203.0.113.10` is a routed public address
- Apply change review before publishing services

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 4 - Major
Residual score: 4
Residual risk : Low
```

### Risk Owner

Security Engineering

---

## R-015: Security Profiles Not Applied

### Risk Description

A firewall policy may allow internet or application traffic without the
required antivirus, IPS, web filtering, DNS filtering, or application
control.

### Potential Impact

- Malware delivery
- Exploit traffic allowed
- Malicious domain access
- Unauthorized applications
- Reduced security visibility

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Define mandatory profile assignments
- Review `utm-status`
- Verify referenced profile names
- Validate traffic and security logs
- Test approved security signatures safely
- Compare firewall policy against the policy matrix
- Review profile changes through change control

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 4 - Major
Residual score: 4
Residual risk : Low
```

### Risk Owner

Security Engineering

---

## R-016: FortiGuard Service Unavailability

### Risk Description

FortiGuard rating, signature, or update services may become temporarily
unavailable.

### Potential Impact

- Outdated security signatures
- Web and DNS rating failures
- Reduced detection capability
- Incorrect category enforcement
- Security-profile errors

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 3 - Moderate
Inherent score : 9
Inherent risk  : Medium
```

### Mitigations

- Monitor FortiGuard connectivity
- Configure expected rating-error behavior
- Maintain valid subscriptions
- Ensure DNS and internet reachability
- Monitor signature-update age
- Alert on update failures
- Document temporary operating procedures

### Residual Risk

```text
Likelihood    : 3 - Possible
Impact        : 3 - Moderate
Residual score: 9
Residual risk : Medium
```

### Risk Owner

Security Operations

---

## R-017: SSL Inspection Compatibility or Privacy Issue

### Risk Description

Deep SSL inspection may break applications, conflict with certificate
pinning, or introduce privacy and legal concerns.

### Potential Impact

- Application failure
- User trust warnings
- Privacy violations
- Increased processing load
- Inspection bypass requests
- Operational disruption

### Assessment

```text
Likelihood     : 4 - Likely
Impact         : 3 - Moderate
Inherent score : 12
Inherent risk  : High
```

### Existing Controls

- Certificate inspection used as the baseline
- Deep inspection not enabled by default
- SSL inspection requirements documented

### Additional Mitigations

- Perform legal and privacy review
- Deploy the inspection CA securely
- Test application compatibility
- Document justified exceptions
- Monitor certificate errors
- Assess firewall capacity
- Protect CA private keys
- Review certificate-pinned applications

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 3 - Moderate
Residual score: 6
Residual risk : Medium
```

### Risk Owner

Security Engineering

---

## R-018: Central Log Server Failure

### Risk Description

The centralized log server may become unavailable or unable to receive logs.

### Potential Impact

- Reduced security visibility
- Missing investigation evidence
- Delayed incident detection
- Lost administrative audit records
- Incomplete compliance evidence

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Monitor `log01` availability
- Monitor log-forwarding status
- Configure local logging where capacity permits
- Protect log-server storage
- Back up critical logs
- Define retention requirements
- Alert on ingestion failure
- Consider redundant logging infrastructure

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Security Operations

---

## R-019: Missing or Incomplete Security Logs

### Risk Description

Required traffic, administrator, UTM, system, HA, or routing events may not be
recorded.

### Potential Causes

- Logging disabled on policies
- Incorrect event filters
- Insufficient storage
- Syslog configuration error
- Incorrect source IP
- Transport incompatibility
- Log filtering too restrictive

### Potential Impact

- Incomplete incident investigations
- Delayed threat detection
- Missing policy evidence
- Inability to trace configuration changes
- Reduced accountability

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Enable traffic logging on important policies
- Enable security-profile logging
- Enable administrator and system events
- Test syslog delivery
- Monitor ingestion volume
- Define retention and backup requirements
- Compare generated events with received events
- Protect logs against modification

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Security Operations

---

## R-020: Time Synchronization Failure

### Risk Description

FortiGate, servers, and security systems may have inconsistent system times.

### Potential Impact

- Incorrect log correlation
- Authentication failures
- Certificate validation problems
- Difficult incident investigation
- Incorrect event sequence

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 3 - Moderate
Inherent score : 9
Inherent risk  : Medium
```

### Mitigations

- Use an approved internal NTP server
- Monitor NTP synchronization
- Restrict NTP access to approved sources
- Validate time after failover
- Alert on significant clock drift
- Use consistent time zones for security operations

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 3 - Moderate
Residual score: 3
Residual risk : Low
```

### Risk Owner

Infrastructure Operations

---

## R-021: Unsupported or Vulnerable FortiOS Version

### Risk Description

The firewall may run a vulnerable, unsupported, or inappropriate FortiOS
version.

### Potential Impact

- Exploitation of known vulnerabilities
- Firewall compromise
- Service instability
- Missing security fixes
- Feature incompatibility

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Mitigations

- Monitor vendor security advisories
- Maintain asset and firmware inventory
- Review release notes
- Validate supported upgrade paths
- Test firmware in a lab
- Back up configuration before upgrade
- Upgrade both HA members according to approved procedures
- Perform post-upgrade validation
- Maintain rollback planning

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 5 - Severe
Residual score: 10
Residual risk : Medium
```

### Risk Owner

Security Engineering

---

## R-022: Configuration Backup Unavailable

### Risk Description

A valid FortiGate configuration backup may not be available during failure,
rollback, or disaster recovery.

### Potential Impact

- Extended outage
- Manual configuration reconstruction
- Incomplete policy recovery
- Increased recovery errors
- Extended security exposure

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Back up before and after approved changes
- Encrypt configuration backups
- Restrict backup access
- Store backups in more than one approved location
- Test restoration procedures
- Label backups with hostname, version, and date
- Avoid publishing configuration backups to GitHub
- Define retention requirements

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 4 - Major
Residual score: 4
Residual risk : Low
```

### Risk Owner

Network Operations

---

## R-023: Unauthorized Configuration Change

### Risk Description

An unauthorized or unapproved change may alter firewall behavior.

### Potential Impact

- Security bypass
- Service outage
- Unauthorized access
- Logging disabled
- Persistence established
- Configuration inconsistency

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Mitigations

- Use individual administrator accounts
- Enable MFA
- Use change approval
- Log all configuration changes
- Review administrator activity
- Back up before changes
- Compare configurations periodically
- Apply least-privilege admin profiles
- Alert on critical policy changes

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 5 - Severe
Residual score: 10
Residual risk : Medium
```

### Risk Owner

Security Engineering

---

## R-024: Sensitive Configuration Exposed in GitHub

### Risk Description

Production credentials, addresses, certificates, usernames, serial numbers,
or confidential architecture information may be uploaded to the public
repository.

### Potential Impact

- Credential compromise
- Infrastructure reconnaissance
- Employer or customer data exposure
- Certificate or key compromise
- Reputational damage
- Security-policy violation

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Existing Controls

- Fictional IP addresses and systems
- Documentation-only public ranges
- Placeholder HA password
- No administrator password
- Sanitized hostnames
- Public environment disclaimer
- `SECURITY.md`

### Additional Mitigations

- Review every commit before publication
- Use secret scanning
- Never upload `show full-configuration` from production
- Remove serial numbers and license information
- Remove real domain names and public IP addresses
- Remove certificates and private keys
- Rotate any credential accidentally exposed
- Rewrite Git history if sensitive data is committed
- Use pull-request review for future changes

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 5 - Severe
Residual score: 5
Residual risk : Low
```

### Risk Owner

Repository Maintainer

---

## R-025: Insufficient Implementation Testing

### Risk Description

A design or configuration may be considered complete without validating
allowed traffic, denied traffic, failover, logging, and security inspection.

### Potential Impact

- Undetected policy errors
- Failed HA operation
- Failed SD-WAN failover
- Missing security profiles
- Application outage
- False portfolio claims

### Assessment

```text
Likelihood     : 4 - Likely
Impact         : 4 - Major
Inherent score : 16
Inherent risk  : High
```

### Existing Controls

- Validation test plan
- Firewall policy validation cases
- Troubleshooting commands
- Project status distinguishes designed and tested controls
- Lab evidence remains pending until execution

### Additional Mitigations

- Execute all formal test cases
- Record actual results
- Capture sanitized evidence
- Test positive and negative scenarios
- Test HA and SD-WAN failure conditions
- Retest after significant changes
- Mark unexecuted tests as `NOT TESTED`
- Do not mark controls as passed without evidence

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Project Owner

---

## R-026: Monitoring Server Compromise

### Risk Description

The monitoring server may be compromised and used to access multiple network
segments or collect sensitive infrastructure information.

### Potential Impact

- Network reconnaissance
- Unauthorized SNMP access
- Credential theft
- Lateral movement
- Monitoring data manipulation
- Reduced security visibility

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Restrict monitoring traffic to `HOST-MONITOR01`
- Explicitly define monitored systems
- Use SNMPv3
- Avoid default community strings
- Harden and patch the monitoring server
- Restrict administrative access
- Apply endpoint protection
- Log monitoring traffic
- Separate monitoring credentials
- Review access periodically

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Security Operations

---

## R-027: DNS Service Disruption or Manipulation

### Risk Description

DNS services may be unavailable, incorrectly configured, or manipulated to
redirect users and systems.

### Potential Impact

- Application-access failure
- Authentication disruption
- Malicious redirection
- Security-update failure
- Public-service disruption

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Restrict clients to approved DNS servers
- Apply DNS filtering
- Block known malicious domains
- Monitor DNS failures
- Log domain requests where appropriate
- Harden the DNS server
- Use separate DMZ DNS architecture in future design
- Protect DNS administration
- Maintain DNS redundancy in an expanded environment

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Infrastructure Operations

---

## R-028: Certificate Expiration or Misconfiguration

### Risk Description

The certificate used by the public application or management interface may
expire, use an incorrect hostname, or have an incomplete trust chain.

### Potential Impact

- User trust warnings
- Application outage
- Failed integrations
- Increased phishing risk
- Administrative-access disruption

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 4 - Major
Inherent score : 12
Inherent risk  : High
```

### Mitigations

- Maintain certificate inventory
- Monitor expiration dates
- Validate certificate hostnames
- Protect private keys
- Use approved certificate authorities
- Test certificate renewal
- Verify complete certificate chains
- Document certificate ownership
- Automate renewal where appropriate

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 4 - Major
Residual score: 8
Residual risk : Medium
```

### Risk Owner

Application Owner

---

## R-029: Network Segmentation Bypass

### Risk Description

Incorrect switching, VLAN tagging, trunking, routing, or firewall interface
configuration may allow traffic to bypass intended security boundaries.

### Potential Impact

- Unauthorized inter-VLAN access
- Guest-network escape
- DMZ isolation failure
- Management-network exposure
- Lateral movement

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Mitigations

- Validate VLAN IDs and subnets
- Restrict switch trunk VLANs
- Avoid unintended Layer 3 routing on the core switch
- Ensure FortiGate is the intended security gateway
- Disable unused switch ports
- Test every inter-zone path
- Validate asymmetric routing
- Document physical and logical connectivity
- Review switch and firewall configurations together

### Residual Risk

```text
Likelihood    : 1 - Rare
Impact        : 5 - Severe
Residual score: 5
Residual risk : Low
```

### Risk Owner

Security Engineering and Network Operations

---

## R-030: Resource Exhaustion or Denial of Service

### Risk Description

High traffic volume, excessive sessions, malicious requests, logging volume,
security inspection, or attack traffic may exhaust firewall or application
resources.

### Potential Impact

- Firewall performance degradation
- Dropped sessions
- Public portal outage
- Delayed security inspection
- Logging failure
- HA failover

### Assessment

```text
Likelihood     : 3 - Possible
Impact         : 5 - Severe
Inherent score : 15
Inherent risk  : High
```

### Mitigations

- Monitor CPU, memory, sessions, and throughput
- Size the FortiGate for inspected traffic
- Apply rate limiting where appropriate
- Use DoS policies
- Implement upstream DDoS protection
- Tune logging volume
- Apply WAF protection
- Create alert thresholds
- Test expected traffic capacity
- Retain sufficient HA capacity

### Residual Risk

```text
Likelihood    : 2 - Unlikely
Impact        : 5 - Severe
Residual score: 10
Residual risk : Medium
```

### Risk Owner

Network and Security Operations

---

# 7. Risk Treatment Summary

## 7.1 Critical Risks

Critical risks requiring treatment before production-style implementation:

- R-001: Overly permissive firewall policy
- R-005: Public DMZ application compromise

These risks require:

- Specific firewall policies
- Security inspection
- Change review
- DMZ segmentation
- Application security testing
- Logging and monitoring
- Evidence-based validation

## 7.2 High Risks

High risks require prioritized mitigation and documented validation:

- Firewall policy ordering
- Unauthorized management access
- Administrator compromise
- DMZ-to-internal access
- Database exposure
- Guest-network isolation
- Firewall and HA failure
- ISP and SD-WAN failure
- NAT and VIP errors
- Missing security profiles
- SSL inspection compatibility
- Logging failure
- Vulnerable FortiOS
- Configuration loss
- Unauthorized changes
- Sensitive GitHub exposure
- Insufficient testing
- Monitoring-server compromise
- DNS issues
- Certificate issues
- Segmentation bypass
- Resource exhaustion

## 7.3 Accepted Medium Risks

The following residual risks may remain after reasonable controls are applied:

- Dependence on external FortiGuard services
- Temporary logging interruption
- SSL-inspection compatibility limitations
- Residual administrator credential risk
- Residual public-application risk
- Residual denial-of-service risk

Acceptance of residual risk should be documented by the relevant owner.

---

# 8. Risk Treatment Options

Each risk may be managed using one or more of the following treatments.

## Mitigate

Implement technical or procedural controls to reduce likelihood or impact.

Examples:

- Restrictive firewall policy
- MFA
- HA
- SD-WAN failover
- IPS
- Central logging

## Avoid

Remove the activity that creates the risk.

Examples:

- Do not expose SSH to the internet
- Do not enable administrative access on WAN interfaces
- Do not allow Guest VLAN access to internal networks

## Transfer

Transfer part of the risk to another party.

Examples:

- ISP service agreement
- Cloud DDoS protection
- Managed security monitoring
- Vendor support contract

## Accept

Formally accept residual risk when additional mitigation is not justified.

Risk acceptance should include:

- Reason for acceptance
- Named owner
- Expiration or review date
- Current controls
- Residual risk rating

---

# 9. Risk Review Triggers

The risk register should be reviewed when:

- A firewall policy is added or significantly modified
- A new VLAN or security zone is introduced
- An internet-facing application is published
- FortiOS is upgraded
- FortiGate hardware or virtual sizing changes
- Security profiles are modified
- HA design changes
- WAN or SD-WAN configuration changes
- A certificate is replaced
- Logging architecture changes
- A security incident occurs
- A vulnerability affects a component
- A configuration is accidentally exposed
- Test results identify unexpected behavior
- The project reaches a new release milestone

---

# 10. Risk Review Checklist

- [ ] All architecture components are represented
- [ ] Firewall design risks have been assessed
- [ ] Management-plane risks have been assessed
- [ ] DMZ risks have been assessed
- [ ] Guest-network risks have been assessed
- [ ] HA risks have been assessed
- [ ] SD-WAN and ISP risks have been assessed
- [ ] Logging risks have been assessed
- [ ] FortiOS vulnerability risks have been assessed
- [ ] Configuration-backup risks have been assessed
- [ ] GitHub exposure risks have been assessed
- [ ] Risk owners have been assigned
- [ ] Existing controls have been documented
- [ ] Additional mitigations have been identified
- [ ] Residual risks have been evaluated
- [ ] Critical risks have treatment plans
- [ ] Lab testing requirements have been linked
- [ ] Risk status is updated after major changes
- [ ] No production information is included

---

# 11. Open Risk Actions

| Action ID | Related Risk | Action | Owner | Priority | Status |
|---|---|---|---|---|---|
| A-001 | R-001 | Validate all firewall policies against the matrix | Security Engineering | High | Planned |
| A-002 | R-005 | Perform public application security testing | Application and Security Teams | High | Planned |
| A-003 | R-009 | Execute Active-Passive HA failover test | Network Operations | High | Pending Lab |
| A-004 | R-012 | Execute primary-to-secondary ISP failover test | Network Operations | High | Pending Lab |
| A-005 | R-015 | Validate all UTM profile assignments | Security Engineering | High | Pending Lab |
| A-006 | R-018 | Validate central syslog delivery | Security Operations | High | Pending Lab |
| A-007 | R-021 | Confirm target FortiOS build and compatibility | Security Engineering | High | Planned |
| A-008 | R-024 | Review repository for secrets and real identifiers | Repository Maintainer | High | In Progress |
| A-009 | R-025 | Execute and document validation test plan | Project Owner | High | Pending Lab |
| A-010 | R-028 | Define certificate lifecycle procedure | Application Owner | Medium | Planned |
| A-011 | R-030 | Review FortiGate capacity and DoS controls | Network Operations | Medium | Planned |

---

# 12. Project Risk Status

| Risk Management Component | Status |
|---|---|
| Initial Risk Identification | Complete |
| Inherent Risk Assessment | Complete |
| Existing Control Mapping | Complete |
| Mitigation Planning | Complete |
| Residual Risk Assessment | Complete |
| Risk Ownership Assignment | Complete |
| Lab Control Validation | Pending Lab Execution |
| Residual Risk Acceptance | Pending Validation |
| Risk Treatment Evidence | Pending Lab Execution |
| Final Risk Review | Pending Project Completion |

> Risk controls are documented as designed controls until they are successfully
> validated in a controlled lab environment.

---

# 13. Related Documentation

- firewall-policy-matrix.md
- implementation-plan.md
- hardening-checklist.md
- test-plan.md
- ../configs/README.md
- ../diagrams/README.md
- ../screenshots/README.md
- ../SECURITY.md

---

# 14. Final Statement

This risk register demonstrates a risk-based approach to enterprise firewall
design.

The presence of a documented control does not automatically mean that the
risk has been fully mitigated. Controls must be:

- Correctly configured
- Independently reviewed
- Functionally tested
- Monitored
- Maintained
- Periodically reassessed

Risks should remain open until the required control has been implemented,
tested, and supported by appropriate evidence.
