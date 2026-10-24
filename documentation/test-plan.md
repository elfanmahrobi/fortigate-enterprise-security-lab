# FortiGate Enterprise Security Lab
## Validation Test Plan

This document defines the functional, security, segmentation, NAT, logging,
SD-WAN, and High Availability tests for the fictional FortiGate Enterprise
Security Lab.

> **Disclaimer**
>
> This test plan is designed for a controlled lab environment.
>
> Testing must not target systems, networks, applications, or services without
> explicit authorization.
>
> All systems, addresses, domains, and organization names are fictional or
> sanitized.

---

# 1. Document Information

| Item | Value |
|---|---|
| Project | FortiGate Enterprise Security Lab |
| Organization | Example Manufacturing Ltd. |
| Environment | Fictional Enterprise Laboratory |
| Document Type | Validation Test Plan |
| Platform | FortiGate NGFW |
| Firmware Baseline | FortiOS 7.6.x |
| Deployment Model | Active-Passive High Availability |
| Internet Design | Dual ISP with SD-WAN |
| Author | Elfan Mahrobi |
| Status | Ready for Lab Execution |
| Classification | Public / Portfolio |

---

# 2. Test Objectives

The test plan aims to verify:

- Interface and VLAN configuration
- Network addressing
- Network segmentation
- Firewall policy behavior
- Source NAT
- Destination NAT
- Virtual IP publishing
- Security-profile assignment
- Guest-network isolation
- DMZ isolation
- Restricted management access
- Centralized logging
- SD-WAN link selection
- SD-WAN failover and recovery
- HA synchronization
- HA failover and recovery
- Configuration backup
- Negative security controls
- Repository evidence sanitization

---

# 3. Test Scope

## 3.1 In Scope

- FortiGate system status
- WAN interfaces
- VLAN interfaces
- Address and service objects
- Security profiles
- Firewall policies
- Local-in policies
- SD-WAN
- Routing
- VIP and DNAT
- Source NAT
- Central logging
- Management access
- HA clustering
- Functional application access
- Safe negative-access testing
- Controlled failover testing

## 3.2 Out of Scope

- Production penetration testing
- Denial-of-service testing
- Destructive exploit testing
- Malware execution
- Credential attacks
- Password spraying
- Public internet scanning
- Unapproved vulnerability scanning
- Production load testing
- Deep SSL inspection
- Remote-access VPN
- Site-to-site VPN
- SAML authentication
- ZTNA
- FortiManager
- FortiAnalyzer

---

# 4. Test Status Values

Use the following status values:

```text
PASS
FAIL
BLOCKED
NOT TESTED
NOT APPLICABLE
```

Definitions:

- `PASS`: The actual result matches the expected result.
- `FAIL`: The actual result does not match the expected result.
- `BLOCKED`: The test cannot proceed because of a documented dependency.
- `NOT TESTED`: The test has not been executed.
- `NOT APPLICABLE`: The test does not apply to the deployed lab.

---

# 5. Severity Values

```text
CRITICAL
HIGH
MEDIUM
LOW
INFORMATIONAL
```

A failure is considered:

- `CRITICAL` when it causes unauthorized public or management exposure
- `HIGH` when network isolation or critical availability fails
- `MEDIUM` when required inspection, logging, or expected access fails
- `LOW` when the issue has limited security or operational impact
- `INFORMATIONAL` when documenting observations or improvement opportunities

---

# 6. Entry Criteria

Testing may begin when:

- [ ] The architecture has been reviewed
- [ ] The addressing plan has been reviewed
- [ ] FortiGate configuration has been applied
- [ ] Required interfaces are up
- [ ] Test endpoints are available
- [ ] Required servers are available
- [ ] FortiGuard licensing has been checked
- [ ] Security profiles have been created
- [ ] Firewall policies have been created
- [ ] SD-WAN has been configured
- [ ] HA cluster status is stable
- [ ] Central logging is available
- [ ] Configuration backup has been completed
- [ ] Test authorization has been recorded
- [ ] Rollback procedure is available

---

# 7. Exit Criteria

Testing is complete when:

- [ ] All critical tests have been executed
- [ ] All critical tests have passed
- [ ] All high-severity failures have been remediated or accepted
- [ ] Allowed traffic has been tested
- [ ] Denied traffic has been tested
- [ ] NAT has been validated
- [ ] Security profiles have been validated
- [ ] Guest isolation has been validated
- [ ] DMZ isolation has been validated
- [ ] Management restriction has been validated
- [ ] Central logging has been validated
- [ ] SD-WAN failover has been tested
- [ ] HA failover has been tested
- [ ] Recovery behavior has been tested
- [ ] Test evidence has been sanitized
- [ ] Test results have been documented
- [ ] Final test summary has been completed

---

# 8. Test Environment

## Network Information

| Network | Subnet | Gateway | Test Purpose |
|---|---|---|---|
| Corporate Users | `10.10.10.0/24` | `10.10.10.1` | User-access testing |
| Internal Servers | `10.10.20.0/24` | `10.10.20.1` | Application and infrastructure testing |
| Guest | `10.10.30.0/24` | `10.10.30.1` | Guest-isolation testing |
| Management | `10.10.40.0/24` | `10.10.40.1` | Administrative testing |
| DMZ | `10.10.50.0/24` | `10.10.50.1` | Public-service testing |
| Security | `10.10.60.0/24` | `10.10.60.1` | Logging and monitoring |
| WAN1 | `203.0.113.0/30` | `203.0.113.1` | Primary ISP |
| WAN2 | `198.51.100.0/30` | `198.51.100.1` | Secondary ISP |

## Systems

| System | Address | Purpose |
|---|---|---|
| `dc01.example.local` | `10.10.20.10` | DNS and identity services |
| `app01.example.local` | `10.10.20.20` | Internal HTTPS application |
| `db01.example.local` | `10.10.20.30` | PostgreSQL database |
| `ntp01.example.local` | `10.10.20.40` | NTP service |
| `web01.example.local` | `10.10.50.10` | Public HTTPS application |
| `admin01.example.local` | `10.10.40.10` | Administrative workstation |
| `log01.example.local` | `10.10.60.10` | Log server |
| `monitor01.example.local` | `10.10.60.20` | Monitoring server |

---

# 9. Evidence Requirements

Each executed test should include:

- Test date and time
- Tester
- FortiOS version and build
- Source address
- Destination address
- Protocol and port
- Expected result
- Actual result
- Test status
- Relevant traffic log
- Relevant security event
- Screenshot or command output
- Issue reference if failed

Evidence must not contain:

- Passwords
- Authentication tokens
- Private keys
- HA passwords
- Serial numbers
- FortiCare information
- Real public addresses
- Production logs
- Employer or customer information

---

# 10. Test Case Template

Use this format for additional tests:

```markdown
## TC-XXX: Test Name

**Category:**  
Category name

**Severity:**  
MEDIUM

**Objective:**  
Describe what the test validates.

**Preconditions:**

- Required configuration
- Required system availability

**Source:**  
Source system and address

**Destination:**  
Destination system and address

**Protocol and service:**  
Protocol and destination port

**Test procedure:**

1. Execute the first step.
2. Execute the second step.
3. Review logs.

**Expected result:**

- Expected behavior
- Expected log
- Expected security action

**Actual result:**  
Not tested.

**Status:**  
`NOT TESTED`

**Evidence:**  
Pending.

**Notes:**  
None.
```

---

# 11. System and Configuration Tests

## TC-001: FortiGate System Health

**Category:** System  
**Severity:** High

**Objective:**  
Verify that the FortiGate is healthy and running the intended FortiOS build.

**Procedure:**

```shell
get system status
get system performance status
```

**Expected Result:**

- Correct FortiOS version is displayed
- System time is correct
- CPU and memory are within acceptable lab limits
- No critical system alarm is present

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-002: Interface Configuration

**Category:** Network  
**Severity:** High

**Objective:**  
Verify WAN, trunk, and VLAN interfaces.

**Procedure:**

```shell
show system interface
get system interface physical
```

**Expected Result:**

- WAN1 uses `203.0.113.2/30`
- WAN2 uses `198.51.100.2/30`
- VLAN gateways match the addressing plan
- Required interfaces are operational
- Administrative access is limited to VLAN40-MGMT

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-003: Firewall Object Validation

**Category:** Configuration  
**Severity:** Medium

**Procedure:**

```shell
show firewall address
show firewall addrgrp
show firewall service custom
show firewall service group
```

**Expected Result:**

- Host and subnet addresses are correct
- Address-group membership is correct
- Custom services use approved ports
- No production information is present

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 12. Corporate User Tests

## TC-010: Corporate HTTPS Internet Access

**Category:** Firewall Policy  
**Severity:** High

**Source:** Corporate test client in VLAN 10  
**Destination:** Approved HTTPS website  
**Service:** TCP/443

**Procedure:**

1. Connect the client to VLAN 10.
2. Confirm the client receives the correct gateway.
3. Access an approved HTTPS website.
4. Review the FortiGate traffic log.
5. Confirm source NAT.

**Expected Result:**

- HTTPS access succeeds
- Source NAT is applied
- Policy `CORP-USERS-to-INTERNET` is matched
- The session is logged
- Security profiles are attached

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-011: Corporate Unauthorized Internet Port

**Category:** Negative Access  
**Severity:** Medium

**Objective:**  
Verify that unauthorized outbound services are denied.

**Source:** Corporate test client  
**Destination:** Authorized test destination  
**Service:** Unapproved test port

**Expected Result:**

- Connection fails
- No broad internet-access policy permits the traffic
- Denied traffic is visible where logging is configured

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-012: Corporate Internal Application Access

**Source:** Corporate test client  
**Destination:** `10.10.20.20`  
**Service:** TCP/443

**Expected Result:**

- Access succeeds
- Policy `CORP-USERS-to-APP01` is matched
- Source NAT is not applied
- Original client IP is preserved
- Session is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-013: Corporate Unauthorized Application Service

**Source:** Corporate test client  
**Destination:** `10.10.20.20`  
**Service:** TCP/22

**Expected Result:**

- Access fails
- HTTPS access remains unaffected
- Denied traffic is logged where configured

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-014: Corporate DNS Access

**Source:** Corporate test client  
**Destination:** `10.10.20.10`  
**Services:** UDP/53 and TCP/53

**Expected Result:**

- Approved DNS requests succeed
- Policy `CORP-USERS-to-DC01-DNS` is matched
- Session is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-015: Corporate NTP Access

**Source:** Corporate test client  
**Destination:** `10.10.20.40`  
**Service:** UDP/123

**Expected Result:**

- Time synchronization succeeds
- Other unapproved services remain blocked

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-016: Corporate Direct Database Access

**Category:** Negative Access  
**Severity:** High

**Source:** Corporate test client  
**Destination:** `10.10.20.30`  
**Service:** TCP/5432

**Expected Result:**

- Database access fails
- Policy `CORP-USERS-to-DB01-DENY` is matched
- Denied session is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 13. Guest Network Tests

## TC-020: Guest Internet Access

**Source:** Guest client in VLAN 30  
**Destination:** Approved HTTPS website  
**Service:** TCP/443

**Expected Result:**

- Internet access succeeds
- Source NAT is applied
- Guest security profiles are applied
- Session is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-021: Guest Access to Corporate Network

**Category:** Network Isolation  
**Severity:** Critical

**Source:** Guest client  
**Destination:** Corporate test client in `10.10.10.0/24`

**Expected Result:**

- Access fails
- Deny policy is matched
- Attempt is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-022: Guest Access to Internal Application

**Source:** Guest client  
**Destination:** `10.10.20.20:443`

**Expected Result:**

- Access fails
- Guest-to-RFC1918 deny policy is matched
- Attempt is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-023: Guest Access to FortiGate Management

**Category:** Management Security  
**Severity:** Critical

**Source:** Guest client  
**Destination:** `10.10.30.1` or another FortiGate interface  
**Services:** HTTPS and SSH

**Expected Result:**

- Administrative access fails
- No GUI or SSH login page is exposed
- Attempt is logged where supported

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 14. DMZ and Public-Service Tests

## TC-030: Public HTTPS Portal

**Category:** DNAT/VIP  
**Severity:** High

**Source:** Authorized external test client  
**Destination:** `203.0.113.10:443`  
**Mapped Destination:** `10.10.50.10:443`

**Expected Result:**

- HTTPS access succeeds
- VIP translation is applied
- Policy `INTERNET-to-DMZ-PORTAL` is matched
- IPS profile is applied
- Session is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-031: Public SSH Access

**Category:** Negative Access  
**Severity:** Critical

**Source:** Authorized external test client  
**Destination:** `203.0.113.10:22`

**Expected Result:**

- SSH access fails
- No SSH service is published
- Attempt is denied and logged where configured

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-032: Public HTTP Access

**Source:** Authorized external test client  
**Destination:** `203.0.113.10:80`

**Expected Result:**

- HTTP access fails unless approved redirection is intentionally configured
- HTTPS remains available

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-033: DMZ Web Server to Database

**Source:** `10.10.50.10`  
**Destination:** `10.10.20.30`  
**Service:** TCP/5432

**Expected Result:**

- Connection succeeds
- Policy `WEB01-to-DB01` is matched
- IPS inspection is applied
- Session is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-034: DMZ Web Server to Database SSH

**Category:** Negative Access  
**Severity:** High

**Source:** `10.10.50.10`  
**Destination:** `10.10.20.30`  
**Service:** TCP/22

**Expected Result:**

- Connection fails
- DMZ deny policy is matched
- Attempt is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-035: Unauthorized DMZ-to-Internal Access

**Category:** Network Isolation  
**Severity:** Critical

**Source:** Unapproved DMZ host  
**Destination:** Internal server  
**Service:** Any unapproved service

**Expected Result:**

- Connection fails
- `DMZ-to-INTERNAL-DENY` is matched
- Attempt is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 15. Management Access Tests

## TC-040: Approved HTTPS Management

**Source:** `10.10.40.10`  
**Destination:** `10.10.40.1`  
**Service:** TCP/443

**Expected Result:**

- FortiGate GUI is reachable
- Administrator trusted host permits the source
- Administrative login is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-041: Approved SSH Management

**Source:** `10.10.40.10`  
**Destination:** `10.10.40.1`  
**Service:** TCP/22

**Expected Result:**

- SSH access succeeds
- Administrator authentication is required
- Login is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-042: Corporate User Management Access

**Category:** Negative Access  
**Severity:** Critical

**Source:** Corporate test client  
**Destination:** FortiGate interface  
**Services:** HTTPS and SSH

**Expected Result:**

- Administrative access fails
- Local-in and interface controls prevent access

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-043: WAN Management Access

**Category:** External Exposure  
**Severity:** Critical

**Source:** Authorized external test client  
**Destination:** WAN1 and WAN2 FortiGate addresses  
**Services:** HTTPS and SSH

**Expected Result:**

- Administrative access fails
- No WAN management service is exposed

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 16. Security Profile Tests

## TC-050: Antivirus Profile Assignment

**Objective:**  
Verify that `AV-CORPORATE` is attached to the correct policies.

**Procedure:**

```shell
show firewall policy
show antivirus profile AV-CORPORATE
```

**Expected Result:**

- Antivirus profile exists
- Corporate and guest internet policies reference the profile
- Malware events are configured for blocking and logging

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-051: Safe Antivirus Validation

**Severity:** High

**Objective:**  
Use an authorized, safe antivirus test mechanism in the isolated lab.

**Expected Result:**

- Test content is blocked
- Antivirus event is logged
- No actual malware is executed

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-052: IPS Profile Assignment

**Procedure:**

```shell
show ips sensor
show firewall policy
```

**Expected Result:**

- `IPS-CORPORATE` is assigned to client traffic
- `IPS-PROTECT-SERVER` is assigned to protected server traffic
- Blocking and logging are enabled

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-053: Web-Filter Validation

**Objective:**  
Access an approved FortiGuard test category from a lab client.

**Expected Result:**

- Approved category is allowed
- Blocked test category is denied
- Web-filter event is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-054: DNS-Filter Validation

**Expected Result:**

- Approved domain resolves
- Approved malicious-domain test is blocked
- DNS-filter event is logged
- No unauthorized target is used

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-055: Application-Control Validation

**Expected Result:**

- Approved business traffic remains functional
- Authorized high-risk application test is blocked or monitored
- Application-control event is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-056: Certificate Inspection

**Expected Result:**

- Valid HTTPS sites remain accessible
- Certificate information is inspected
- Invalid-certificate behavior matches policy
- Deep inspection is not unintentionally active

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 17. Logging and Monitoring Tests

## TC-060: Traffic Log Generation

**Procedure:**

1. Generate approved corporate HTTPS traffic.
2. Open FortiGate traffic logs.
3. Search for the client address and policy ID.

**Expected Result:**

- Source and destination are visible
- Policy name is visible
- NAT details are visible
- Action is recorded

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-061: Denied Traffic Logging

**Procedure:**

1. Attempt guest access to an internal server.
2. Review denied traffic logs.

**Expected Result:**

- Denied source and destination are visible
- Matching deny policy is visible
- Timestamp is correct

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-062: Syslog Forwarding

**Source:** FortiGate  
**Destination:** `10.10.60.10:6514`

**Expected Result:**

- FortiGate sends Syslog traffic
- `log01` receives the event
- Timestamp and hostname are correct

**Validation Command:**

```shell
diagnose sniffer packet any 'host 10.10.60.10 and port 6514' 4 0 l
```

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-063: Configuration Change Event

**Procedure:**

1. Perform an approved non-disruptive configuration change.
2. Revert the test change.
3. Review FortiGate and central logs.

**Expected Result:**

- Administrator name is logged
- Change time is logged
- Configuration event reaches `log01`

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 18. SD-WAN Tests

## TC-070: Normal WAN Selection

**Expected Result:**

- WAN1 is healthy
- WAN2 is healthy
- WAN1 is selected as preferred
- Internet access succeeds

**Commands:**

```shell
diagnose sys sdwan health-check
diagnose sys sdwan service
get router info routing-table all
```

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-071: WAN1 Failure

**Category:** Availability  
**Severity:** High

**Procedure:**

1. Confirm WAN1 is active.
2. Begin continuous connectivity testing.
3. Disconnect or disable WAN1 in the lab.
4. Observe the SLA status.
5. Validate internet access through WAN2.
6. Review logs.

**Expected Result:**

- WAN1 failure is detected
- WAN2 becomes active
- Internet connectivity recovers
- Failover event is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-072: WAN1 Recovery

**Procedure:**

1. Restore WAN1.
2. Observe the SLA status.
3. Confirm WAN1 becomes healthy.
4. Confirm path selection follows configured priority.
5. Review recovery logs.

**Expected Result:**

- WAN1 becomes healthy
- Routing returns to the intended state
- Recovery event is logged
- No persistent routing loop occurs

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-073: WAN2 Failure During Normal Operation

**Expected Result:**

- WAN1 remains active
- User traffic is not disrupted
- WAN2 failure is detected and logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 19. High-Availability Tests

## TC-080: HA Cluster Health

**Commands:**

```shell
get system ha status
diagnose sys ha status
```

**Expected Result:**

- Two members are visible
- FGT-EDGE-01 is the preferred primary
- FGT-EDGE-02 is secondary
- Configuration is synchronized
- Heartbeat interfaces are operational

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-081: Primary Firewall Failure

**Category:** Availability  
**Severity:** Critical

**Procedure:**

1. Confirm cluster health.
2. Start continuous connectivity tests.
3. Simulate primary failure in the controlled lab.
4. Observe cluster status.
5. Validate critical traffic.
6. Review logs.

**Expected Result:**

- FGT-EDGE-02 becomes active
- Critical connectivity recovers
- No split-brain condition occurs
- HA event is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-082: Post-Failover Service Validation

After failover, validate:

- Corporate internet access
- Internal application access
- Guest internet access
- Guest isolation
- Public HTTPS portal
- DMZ-to-database access
- FortiGate management
- Centralized logging

**Expected Result:**

All approved services operate through the active secondary firewall.

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-083: Primary Firewall Recovery

**Expected Result:**

- Restored member rejoins the cluster
- Configuration synchronizes
- Final roles match the intended priority and override behavior
- No split-brain condition occurs
- Recovery event is logged

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-084: HA Monitored Interface Failure

**Objective:**  
Verify failover behavior when a monitored interface fails.

**Expected Result:**

- The cluster detects the monitored-interface failure
- Failover behavior follows the HA configuration
- Event is logged
- Service validation succeeds after failover

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 20. Backup and Recovery Tests

## TC-090: Configuration Backup

**Expected Result:**

- Configuration backup completes
- Backup is stored securely
- Backup is not uploaded to GitHub
- Filename identifies device and date

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-091: Backup Readability

**Objective:**  
Verify that the backup file is available and readable by authorized
administrators.

**Expected Result:**

- Backup file is accessible
- File is not empty or corrupted
- Access is restricted

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

## TC-092: Recovery Procedure Review

**Objective:**  
Review the documented restoration process without disrupting the active lab.

**Expected Result:**

- Console access method is documented
- Restore steps are documented
- Rollback owner is defined
- Required files are available

**Actual Result:** Not tested  
**Status:** `NOT TESTED`  
**Evidence:** Pending

---

# 21. Test Summary Matrix

| Test ID | Test Name | Severity | Status |
|---|---|---|---|
| TC-001 | FortiGate System Health | High | Not Tested |
| TC-002 | Interface Configuration | High | Not Tested |
| TC-003 | Firewall Object Validation | Medium | Not Tested |
| TC-010 | Corporate HTTPS Internet Access | High | Not Tested |
| TC-011 | Corporate Unauthorized Internet Port | Medium | Not Tested |
| TC-012 | Corporate Internal Application Access | High | Not Tested |
| TC-013 | Corporate Unauthorized Application Service | Medium | Not Tested |
| TC-014 | Corporate DNS Access | Medium | Not Tested |
| TC-015 | Corporate NTP Access | Low | Not Tested |
| TC-016 | Corporate Direct Database Access | High | Not Tested |
| TC-020 | Guest Internet Access | Medium | Not Tested |
| TC-021 | Guest Access to Corporate Network | Critical | Not Tested |
| TC-022 | Guest Access to Internal Application | High | Not Tested |
| TC-023 | Guest Access to FortiGate Management | Critical | Not Tested |
| TC-030 | Public HTTPS Portal | High | Not Tested |
| TC-031 | Public SSH Access | Critical | Not Tested |
| TC-032 | Public HTTP Access | Medium | Not Tested |
| TC-033 | DMZ Web Server to Database | High | Not Tested |
| TC-034 | DMZ Web Server to Database SSH | High | Not Tested |
| TC-035 | Unauthorized DMZ-to-Internal Access | Critical | Not Tested |
| TC-040 | Approved HTTPS Management | High | Not Tested |
| TC-041 | Approved SSH Management | High | Not Tested |
| TC-042 | Corporate User Management Access | Critical | Not Tested |
| TC-043 | WAN Management Access | Critical | Not Tested |
| TC-050 | Antivirus Profile Assignment | Medium | Not Tested |
| TC-051 | Safe Antivirus Validation | High | Not Tested |
| TC-052 | IPS Profile Assignment | Medium | Not Tested |
| TC-053 | Web-Filter Validation | Medium | Not Tested |
| TC-054 | DNS-Filter Validation | Medium | Not Tested |
| TC-055 | Application-Control Validation | Medium | Not Tested |
| TC-056 | Certificate Inspection | Medium | Not Tested |
| TC-060 | Traffic Log Generation | Medium | Not Tested |
| TC-061 | Denied Traffic Logging | Medium | Not Tested |
| TC-062 | Syslog Forwarding | High | Not Tested |
| TC-063 | Configuration Change Event | High | Not Tested |
| TC-070 | Normal WAN Selection | High | Not Tested |
| TC-071 | WAN1 Failure | High | Not Tested |
| TC-072 | WAN1 Recovery | High | Not Tested |
| TC-073 | WAN2 Failure During Normal Operation | Medium | Not Tested |
| TC-080 | HA Cluster Health | Critical | Not Tested |
| TC-081 | Primary Firewall Failure | Critical | Not Tested |
| TC-082 | Post-Failover Service Validation | Critical | Not Tested |
| TC-083 | Primary Firewall Recovery | High | Not Tested |
| TC-084 | HA Monitored Interface Failure | High | Not Tested |
| TC-090 | Configuration Backup | High | Not Tested |
| TC-091 | Backup Readability | Medium | Not Tested |
| TC-092 | Recovery Procedure Review | Medium | Not Tested |

---

# 22. Defect Template

```markdown
## DEF-XXX: Defect Title

**Related test:** TC-XXX

**Severity:** HIGH

**Description:**  
Describe the difference between the expected and actual result.

**Expected result:**  
Describe the expected behavior.

**Actual result:**  
Describe the observed behavior.

**Affected components:**

- Firewall policy
- Interface
- Security profile
- Application

**Evidence:**  
Add sanitized evidence path.

**Temporary workaround:**  
Describe the workaround, if available.

**Recommended remediation:**  
Describe the corrective action.

**Owner:**  
Assign an owner.

**Target date:**  
YYYY-MM-DD

**Status:**  
OPEN
```

---

# 23. Troubleshooting Commands

## System

```shell
get system status
get system performance status
get system interface physical
```

## Routing

```shell
get router info routing-table all
get router info routing-table details
diagnose ip route list
```

## SD-WAN

```shell
diagnose sys sdwan health-check
diagnose sys sdwan service
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

Always stop debugging:

```shell
diagnose debug disable
diagnose debug reset
```

## HA

```shell
get system ha status
diagnose sys ha status
```

---

# 24. Final Test Report

```text
Test Start Date      :
Test End Date        :
Tester               :
FortiGate Model      :
FortiOS Version      :
FortiOS Build        :
Total Tests          :
Passed               :
Failed               :
Blocked              :
Not Tested           :
Critical Failures    :
High Failures        :
```

## Final Decision

Select one:

```text
[ ] PASS
[ ] PASS WITH EXCEPTIONS
[ ] FAIL
[ ] TESTING INCOMPLETE
[ ] NOT YET EXECUTED
```

## Final Notes

```text
Add the test conclusion, accepted risks, failed controls, and required
remediation here.
```

---

# 25. Current Test Status

This test plan has been prepared but has not yet been executed.

```text
Current Status: NOT YET EXECUTED
```

No test must be marked as passed until:

- The relevant configuration has been applied
- The test has been executed
- The result has been observed
- Supporting evidence has been collected
- The evidence has been sanitized
- The result has been documented

---

# 26. References

- Project Architecture Diagram
- Project Firewall Policy Matrix
- Project Implementation Plan
- Project Security Hardening Checklist
- FortiOS Administration Guide
- FortiOS CLI Reference
- FortiOS Best Practices

---

# 27. Final Disclaimer

The presence of a test case does not establish that a security control is
operational.

Only completed tests with valid evidence should be reported as passed.

Testing must use controlled, safe, and authorized methods. The test plan must
not be used against systems or networks without explicit authorization.
