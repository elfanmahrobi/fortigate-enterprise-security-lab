# FortiGate Enterprise Security Lab
## Security Hardening Checklist

This document defines the security-hardening requirements for the fictional
FortiGate Enterprise Security Lab.

The checklist covers the FortiGate management plane, network interfaces,
firewall policies, security profiles, logging, high availability, SD-WAN,
configuration protection, firmware management, and operational security.

> **Disclaimer**
>
> This checklist was created exclusively for educational and portfolio
> purposes.
>
> All systems, addresses, domains, configurations, and organization names are
> fictional or sanitized.
>
> The checklist must be adapted to the FortiGate model, FortiOS build,
> licensing, operating environment, security policy, and regulatory
> requirements before use.

---

# 1. Document Information

| Item | Value |
|---|---|
| Project | FortiGate Enterprise Security Lab |
| Organization | Example Manufacturing Ltd. |
| Environment | Fictional Enterprise Laboratory |
| Document Type | Security Hardening Checklist |
| Platform | FortiGate NGFW |
| Firmware Baseline | FortiOS 7.6.x |
| Deployment Model | Active-Passive High Availability |
| Author | Elfan Mahrobi |
| Status | Ready for Lab Review |
| Classification | Public / Portfolio |

---

# 2. Hardening Objectives

The hardening process aims to:

- Reduce the FortiGate attack surface
- Restrict access to the management plane
- Apply least-privilege administration
- Protect administrator credentials
- Disable unnecessary services
- Enforce secure management protocols
- Strengthen firewall-policy controls
- Protect internet-bound and inbound traffic
- Isolate guest and DMZ networks
- Secure high-availability communication
- Protect logs and configuration backups
- Maintain supported and secure firmware
- Detect unauthorized configuration changes
- Support incident response and recovery

---

# 3. Hardening Status Values

Use the following values when reviewing each control:

```text
PASS
FAIL
PARTIAL
NOT APPLICABLE
NOT TESTED
```

Do not mark a control as `PASS` without supporting configuration or test
evidence.

---

# 4. Administrative Account Security

- [ ] Default administrator credentials have been changed
- [ ] The default administrator account is renamed or restricted
- [ ] Individual administrator accounts are used
- [ ] Shared administrator accounts are prohibited
- [ ] Strong administrator passwords are required
- [ ] Administrator passwords are not stored in GitHub
- [ ] Administrator passwords are not included in configuration evidence
- [ ] Multi-factor authentication is enabled where available
- [ ] Administrator trusted hosts are configured
- [ ] Administrator access profiles follow least privilege
- [ ] Read-only administrator profiles are used for monitoring roles
- [ ] Super-administrator privileges are limited
- [ ] Unused administrator accounts are disabled or removed
- [ ] Temporary administrator accounts have expiration dates
- [ ] Administrator account ownership is documented
- [ ] Failed administrator logins are logged
- [ ] Successful administrator logins are logged
- [ ] Administrator lockout settings have been reviewed
- [ ] Password-reuse controls have been reviewed
- [ ] Secure password hashing options have been reviewed
- [ ] Administrative session timeout is configured
- [ ] Administrative sessions are closed after use

## Lab Validation

```text
Approved administrator : secadmin
Approved source         : 10.10.40.10
Approved network        : VLAN40-MGMT
Required MFA            : Recommended
Session timeout         : 10 minutes
```

## Evidence

```text
show system admin
show system accprofile
show system global
```

## Review Result

```text
Status        :
Reviewer      :
Review Date   :
Evidence Path :
Notes         :
```

---

# 5. Management Network Security

- [ ] A dedicated Management VLAN is configured
- [ ] Management traffic is separated from user traffic
- [ ] Management traffic is separated from guest traffic
- [ ] Management traffic is separated from DMZ traffic
- [ ] Management access is not enabled on WAN interfaces
- [ ] Management access is not enabled on the Guest interface
- [ ] Management access is not enabled on the DMZ interface
- [ ] Management access is restricted to approved source addresses
- [ ] Local-in policies restrict access to FortiGate services
- [ ] Administrator trusted hosts are configured
- [ ] Console access is physically or logically protected
- [ ] Out-of-band access is available for recovery where possible
- [ ] Management traffic is logged
- [ ] Unauthorized management attempts are logged
- [ ] Management DNS and NTP dependencies are documented

## Approved Design

```text
Management VLAN     : VLAN40-MGMT
Management Subnet   : 10.10.40.0/24
FortiGate Address   : 10.10.40.1
Admin Workstation   : 10.10.40.10
Permitted Services  : HTTPS, SSH, PING
```

## Evidence

```text
show system interface VLAN40-MGMT
show firewall local-in-policy
show system admin
```

---

# 6. Administrative Protocol Security

- [ ] HTTPS is used for GUI administration
- [ ] SSH is used for CLI administration
- [ ] HTTP administrative access is disabled
- [ ] Telnet administrative access is disabled
- [ ] Unnecessary SNMP access is disabled
- [ ] Administrative access uses TLS 1.2 or TLS 1.3
- [ ] Legacy TLS versions are disabled
- [ ] Weak cryptographic algorithms have been reviewed
- [ ] The FortiGate administration certificate is valid
- [ ] The certificate name matches the management hostname where applicable
- [ ] Certificate expiration is monitored
- [ ] SSH access is restricted to approved administrators
- [ ] Non-standard management ports are considered only when justified
- [ ] Changing management ports is not treated as a replacement for access control

## Evidence

```text
show system global
show system interface
show system admin
show firewall local-in-policy
```

---

# 7. Interface Hardening

- [ ] Interface roles are correctly assigned
- [ ] Administrative access is disabled on untrusted interfaces
- [ ] Only required protocols are enabled under `allowaccess`
- [ ] Unused physical interfaces are disabled where practical
- [ ] Unused switch ports are disabled
- [ ] Interface aliases describe their functions
- [ ] WAN interfaces permit only required local services
- [ ] VLAN parent interface is not used for ordinary endpoint access
- [ ] VLAN IDs are documented
- [ ] No overlapping subnets exist
- [ ] Interface addressing matches the approved design
- [ ] Link monitoring is configured for critical interfaces
- [ ] Interface status changes are logged
- [ ] DHCP services are enabled only where required
- [ ] Device identification is enabled only when required
- [ ] LLDP exposure has been reviewed
- [ ] IPv6 is disabled or secured if not required

## Expected Interface Access

```text
wan1              : PING only if operationally required
wan2              : PING only if operationally required
VLAN10-USERS      : PING
VLAN20-SERVERS    : PING
VLAN30-GUEST      : PING
VLAN40-MGMT       : PING, HTTPS, SSH
VLAN50-DMZ        : PING
VLAN60-SECURITY   : PING
```

## Evidence

```text
show system interface
get system interface physical
```

---

# 8. Network Segmentation

- [ ] Corporate users are placed in VLAN 10
- [ ] Internal servers are placed in VLAN 20
- [ ] Guest devices are placed in VLAN 30
- [ ] Management systems are placed in VLAN 40
- [ ] Public services are placed in VLAN 50
- [ ] Security systems are placed in VLAN 60
- [ ] Inter-VLAN traffic is inspected by FortiGate
- [ ] Network segmentation follows least privilege
- [ ] Guest access to RFC1918 networks is denied
- [ ] Corporate users cannot directly access the database
- [ ] DMZ access to internal servers is explicitly restricted
- [ ] Management access originates only from approved systems
- [ ] Monitoring traffic originates only from the monitoring server
- [ ] Logging traffic is restricted to the central log server
- [ ] The implicit deny remains effective
- [ ] Explicit deny policies are used where visibility is required

## Evidence

```text
show system interface
show firewall policy
show firewall address
show firewall addrgrp
```

---

# 9. Firewall Policy Hardening

- [ ] Each policy has a unique and descriptive name
- [ ] Source interfaces are specific
- [ ] Destination interfaces are specific
- [ ] Source addresses are restricted
- [ ] Destination addresses are restricted
- [ ] Required services are specific
- [ ] Use of the `ALL` service is minimized
- [ ] Use of the `all` address object is minimized
- [ ] Policy actions are correct
- [ ] NAT is enabled only where required
- [ ] Internal traffic preserves the original source IP where possible
- [ ] Security profiles are assigned where required
- [ ] Logging is enabled
- [ ] Policy comments contain ownership and purpose
- [ ] Temporary policies have expiration dates
- [ ] Disabled policies are reviewed and removed when no longer required
- [ ] Duplicate policies are removed
- [ ] Shadowed policies are reviewed
- [ ] Policy ordering has been validated
- [ ] Internet-access policies allow only approved services
- [ ] Public inbound policies expose only required ports
- [ ] DMZ-to-internal policies use host-specific objects
- [ ] Database policies use port-specific services
- [ ] Denied traffic is logged where operationally useful
- [ ] Firewall policies are periodically recertified

## Policy Comment Template

```text
Purpose:
Business owner:
Technical owner:
Change reference:
Implementation date:
Review date:
Expiration date:
Risk classification:
```

## Evidence

```text
show firewall policy
diagnose firewall iprope show 100004
```

---

# 10. Firewall Object Hardening

- [ ] Address objects use descriptive names
- [ ] Host objects use `/32` addresses
- [ ] Network objects use correct subnet masks
- [ ] Address groups contain only approved members
- [ ] Service objects use the minimum required ports
- [ ] Service groups contain only approved services
- [ ] Duplicate address objects are removed
- [ ] Duplicate service objects are removed
- [ ] Unused objects are reviewed
- [ ] Object comments identify purpose and ownership where applicable
- [ ] No real production addresses are included in the public repository
- [ ] No confidential domain names are included
- [ ] Public documentation ranges are used for dummy WAN addresses

## Evidence

```text
show firewall address
show firewall addrgrp
show firewall service custom
show firewall service group
```

---

# 11. NAT and Virtual IP Hardening

- [ ] Source NAT is enabled only for approved outbound policies
- [ ] Destination NAT is limited to approved public services
- [ ] The public VIP exposes TCP/443 only
- [ ] SSH is not published to the internet
- [ ] RDP is not published to the internet
- [ ] Administrative interfaces are not included in VIP configuration
- [ ] The mapped server is located in the DMZ
- [ ] Inbound traffic receives IPS inspection
- [ ] Inbound sessions are logged
- [ ] The routed public-IP dependency is documented
- [ ] VIP ownership is documented
- [ ] Unused VIP objects are removed
- [ ] The public application certificate is monitored
- [ ] The public application is tested for vulnerabilities
- [ ] WAF protection is considered for production designs

## Approved Translation

```text
VIP name        : VIP-PORTAL-HTTPS
External IP     : 203.0.113.10
External port   : TCP/443
Mapped IP       : 10.10.50.10
Mapped port     : TCP/443
```

## Evidence

```text
show firewall vip
show firewall policy
```

---

# 12. Antivirus Hardening

- [ ] Antivirus protection is enabled on applicable policies
- [ ] The antivirus profile uses the intended inspection mode
- [ ] Supported protocols are scanned
- [ ] Malware is blocked rather than monitored only
- [ ] Antivirus events are logged
- [ ] FortiGuard antivirus signatures are current
- [ ] File-size limitations are reviewed
- [ ] Oversized-file behavior is documented
- [ ] Encrypted traffic limitations are understood
- [ ] Antivirus test results are documented

## Expected Profile

```text
Profile name    : AV-CORPORATE
Inspection mode : Flow-based
Action          : Block
Logging         : Enabled
```

## Evidence

```text
show antivirus profile AV-CORPORATE
diagnose autoupdate versions
```

---

# 13. IPS Hardening

- [ ] IPS is enabled on relevant traffic paths
- [ ] Corporate client traffic uses `IPS-CORPORATE`
- [ ] Internet-facing traffic uses `IPS-PROTECT-SERVER`
- [ ] Critical signatures are blocked
- [ ] High-severity signatures are blocked
- [ ] Medium-severity server signatures are reviewed
- [ ] IPS events are logged
- [ ] Packet logging is considered for critical events
- [ ] IPS signatures are current
- [ ] IPS exceptions are documented
- [ ] False positives are investigated before creating exemptions
- [ ] Broad IPS exemptions are prohibited
- [ ] Server-protection profiles are tuned to the protected services
- [ ] IPS performance impact has been reviewed
- [ ] Safe validation tests have been completed

## Evidence

```text
show ips sensor
diagnose autoupdate versions
```

---

# 14. Web-Filtering Hardening

- [ ] Web filtering is enabled on internet-access policies
- [ ] Corporate and guest profiles are separated
- [ ] Malicious websites are blocked
- [ ] Phishing sites are blocked
- [ ] Proxy-avoidance services are restricted
- [ ] Peer-to-peer categories are restricted
- [ ] Cryptocurrency-mining categories are restricted
- [ ] Newly observed domains are reviewed
- [ ] Web-filter violations are logged
- [ ] Business exceptions are documented
- [ ] Exemptions have owners and review dates
- [ ] Broad URL exemptions are prohibited
- [ ] Replacement messages do not disclose sensitive information
- [ ] FortiGuard web-filter connectivity is verified

## Evidence

```text
show webfilter profile
diagnose debug rating
```

---

# 15. DNS-Filtering Hardening

- [ ] DNS filtering is enabled on applicable internet policies
- [ ] Botnet command-and-control blocking is enabled
- [ ] Malicious domains are blocked
- [ ] Phishing domains are blocked
- [ ] DNS-filter events are logged
- [ ] Internal clients use approved DNS resolvers
- [ ] Unauthorized DNS resolvers are restricted where required
- [ ] External DNS-over-HTTPS behavior is reviewed
- [ ] DNS exceptions are documented
- [ ] FortiGuard DNS-rating connectivity is verified
- [ ] DNS-filter behavior is tested safely

## Evidence

```text
show dnsfilter profile
diagnose debug rating
```

---

# 16. Application-Control Hardening

- [ ] Application control is enabled on internet-access policies
- [ ] Corporate and guest application profiles are separated
- [ ] Unauthorized remote-access tools are restricted
- [ ] Peer-to-peer applications are restricted
- [ ] Public proxy applications are restricted
- [ ] Anonymizer applications are restricted
- [ ] Cryptocurrency-mining applications are restricted
- [ ] High-risk applications are reviewed
- [ ] Unknown applications are logged
- [ ] Approved business applications are documented
- [ ] Application exceptions have owners and expiration dates
- [ ] Application-control category IDs have been validated
- [ ] Application signatures are current

## Evidence

```text
show application list
diagnose autoupdate versions
```

---

# 17. SSL and Certificate Inspection

- [ ] Certificate inspection is enabled on approved policies
- [ ] Deep SSL inspection is not enabled unintentionally
- [ ] Deep inspection requires formal approval
- [ ] FortiGate CA deployment requirements are documented
- [ ] Privacy and legal requirements are considered
- [ ] Certificate-pinned applications are identified
- [ ] Banking, health, and other sensitive categories are reviewed
- [ ] Certificate-expiration monitoring is configured
- [ ] Invalid-certificate behavior is documented
- [ ] Weak TLS versions are restricted where supported
- [ ] Certificate private keys are protected
- [ ] Certificate private keys are never committed to GitHub
- [ ] Certificate inspection has been tested

## Evidence

```text
show firewall ssl-ssh-profile
show vpn certificate local
```

---

# 18. Guest Network Hardening

- [ ] Guest users are isolated from corporate users
- [ ] Guest users are isolated from internal servers
- [ ] Guest users are isolated from management systems
- [ ] Guest users are isolated from security systems
- [ ] Guest users cannot access FortiGate administration
- [ ] Guest users can access only approved internet services
- [ ] Guest traffic receives security inspection
- [ ] Guest sessions are logged
- [ ] Guest-to-RFC1918 traffic is explicitly denied
- [ ] Peer-to-peer applications are restricted
- [ ] Guest bandwidth limits are considered
- [ ] Guest session limits are considered
- [ ] Guest DNS usage is controlled

## Evidence

```text
show firewall policy
diagnose sys session list
```

---

# 19. DMZ Hardening

- [ ] Public services are placed in the DMZ
- [ ] The DMZ is separated from internal servers
- [ ] Internet access exposes only approved services
- [ ] Internet-to-DMZ HTTPS is inspected
- [ ] DMZ-to-database traffic is host-specific
- [ ] DMZ-to-database traffic is port-specific
- [ ] Remaining DMZ-to-internal traffic is denied
- [ ] DMZ administrative services are not internet-facing
- [ ] DMZ systems use secure DNS and NTP services
- [ ] DMZ server patching is documented
- [ ] DMZ server endpoint protection is considered
- [ ] DMZ server logs are centralized
- [ ] Public application certificates are monitored
- [ ] Vulnerability assessment is performed
- [ ] WAF protection is considered
- [ ] DMZ compromise scenarios are included in incident-response planning

## Evidence

```text
show firewall vip
show firewall policy
```

---

# 20. SD-WAN Hardening

- [ ] WAN1 and WAN2 addressing is correct
- [ ] Upstream gateways are correct
- [ ] Health-check targets are approved
- [ ] Multiple health-check targets are used where appropriate
- [ ] WAN1 is the preferred member
- [ ] WAN2 is the secondary member
- [ ] SLA thresholds are documented
- [ ] Failed links are removed from forwarding
- [ ] Recovered-link behavior is documented
- [ ] SD-WAN rules follow least privilege where applicable
- [ ] Default routing does not conflict with SD-WAN
- [ ] SD-WAN events are logged
- [ ] WAN failover has been tested
- [ ] WAN recovery has been tested
- [ ] Asymmetric routing risks have been reviewed

## Evidence

```text
show system sdwan
diagnose sys sdwan health-check
diagnose sys sdwan service
get router info routing-table all
```

---

# 21. High-Availability Hardening

- [ ] HA uses dedicated heartbeat interfaces
- [ ] Two heartbeat links are configured
- [ ] The cluster uses a strong HA password
- [ ] The HA password is not stored in GitHub
- [ ] Both devices use the same FortiOS version and build
- [ ] Cluster name and group ID are documented
- [ ] Primary and secondary priorities are documented
- [ ] Session pickup is configured
- [ ] Critical interfaces are monitored
- [ ] Configuration synchronization is healthy
- [ ] Split-brain prevention has been reviewed
- [ ] HA management access has been reviewed
- [ ] HA events are centrally logged
- [ ] Primary failure has been tested
- [ ] Primary recovery has been tested
- [ ] Configuration backup is completed after forming the cluster

## Evidence

```text
get system ha status
diagnose sys ha status
show system ha
```

---

# 22. Logging and Monitoring Hardening

- [ ] Traffic logging is enabled on approved policies
- [ ] Denied traffic is logged where required
- [ ] Antivirus events are logged
- [ ] IPS events are logged
- [ ] Web-filter events are logged
- [ ] DNS-filter events are logged
- [ ] Application-control events are logged
- [ ] Administrator logins are logged
- [ ] Failed administrator logins are logged
- [ ] Configuration changes are logged
- [ ] HA events are logged
- [ ] SD-WAN events are logged
- [ ] Interface events are logged
- [ ] Routing events are logged
- [ ] Logs are forwarded to the central log server
- [ ] Log transport is protected
- [ ] Log-server access is restricted
- [ ] System time is synchronized
- [ ] Log retention requirements are documented
- [ ] Log forwarding failure is monitored
- [ ] Sensitive data exposure in logs is reviewed

## Approved Destination

```text
Server    : log01.example.local
Address   : 10.10.60.10
Port      : TCP/6514
Source IP : 10.10.60.1
```

## Evidence

```text
show log syslogd setting
show log eventfilter
execute ping 10.10.60.10
```

---

# 23. Time Synchronization

- [ ] FortiGate time is correct
- [ ] The correct timezone is configured
- [ ] NTP synchronization is enabled
- [ ] The approved NTP server is configured
- [ ] NTP reachability is verified
- [ ] Internal systems use a consistent time source
- [ ] HA members have synchronized time
- [ ] Log timestamps are consistent
- [ ] NTP configuration changes are logged

## Evidence

```text
get system status
show system ntp
execute ping 10.10.20.40
```

---

# 24. Firmware and Vulnerability Management

- [ ] FortiOS model and build are documented
- [ ] Firmware is obtained from an authorized source
- [ ] Release notes are reviewed before upgrades
- [ ] Known issues are reviewed before upgrades
- [ ] PSIRT advisories are monitored
- [ ] The approved upgrade path is reviewed
- [ ] Compatibility with integrated products is reviewed
- [ ] Configuration backup is completed before upgrade
- [ ] HA upgrade requirements are reviewed
- [ ] Upgrade rollback procedures are documented
- [ ] Post-upgrade validation is completed
- [ ] FortiGuard engines and databases are current
- [ ] Emergency security updates follow change control
- [ ] Unsupported firmware is not used for production designs

## Evidence

```text
get system status
diagnose autoupdate versions
```

---

# 25. Configuration Backup and Recovery

- [ ] A baseline backup exists
- [ ] A backup is created before major changes
- [ ] A backup is created after successful implementation
- [ ] Backups are stored securely
- [ ] Backup access is restricted
- [ ] Backup files are encrypted where appropriate
- [ ] Backup files are not committed to GitHub
- [ ] Backup filenames include device and date information
- [ ] Restore procedures are documented
- [ ] Recovery console access is available
- [ ] Backup restoration is periodically tested
- [ ] HA backup and restore considerations are documented
- [ ] Configuration differences are reviewed after restoration

## Suggested Naming

```text
FGT-ENTERPRISE-HA_YYYY-MM-DD_pre-change.conf
FGT-ENTERPRISE-HA_YYYY-MM-DD_post-change.conf
```

---

# 26. Physical and Environmental Security

- [ ] Physical FortiGate access is restricted
- [ ] Console ports are protected
- [ ] Network cabinets are locked
- [ ] Power redundancy is available where required
- [ ] UPS protection is available
- [ ] HA devices do not share a single avoidable failure point
- [ ] WAN and heartbeat cables are labeled
- [ ] Device tampering is monitored
- [ ] Asset ownership is documented
- [ ] Device disposal procedures protect configuration data

---

# 27. Repository and Documentation Security

- [ ] No production configuration is uploaded
- [ ] No real public IP address is uploaded
- [ ] No credential is uploaded
- [ ] No private key is uploaded
- [ ] No authentication token is uploaded
- [ ] No HA password is uploaded
- [ ] No certificate private key is uploaded
- [ ] No FortiCare information is uploaded
- [ ] No serial number is uploaded
- [ ] No employer data is uploaded
- [ ] No customer data is uploaded
- [ ] No production log is uploaded
- [ ] Screenshots are sanitized
- [ ] Dummy domains are used
- [ ] Documentation IP ranges are used
- [ ] Git history is reviewed for accidental secrets
- [ ] Repository security policy is available
- [ ] Repository disclaimer is visible

---

# 28. Validation Commands

```shell
get system status
get system performance status
show system interface
get system interface physical
show system admin
show system accprofile
show firewall local-in-policy
show firewall address
show firewall addrgrp
show firewall service custom
show firewall service group
show firewall vip
show firewall policy
show antivirus profile
show ips sensor
show webfilter profile
show dnsfilter profile
show application list
show firewall ssl-ssh-profile
show system sdwan
diagnose sys sdwan health-check
diagnose sys sdwan service
get router info routing-table all
show log syslogd setting
show log eventfilter
get system ha status
diagnose sys ha status
diagnose autoupdate versions
```

---

# 29. Hardening Review Summary

| Category | Status | Evidence | Notes |
|---|---|---|---|
| Administrator Security | Not Tested | Pending | |
| Management Network | Not Tested | Pending | |
| Administrative Protocols | Not Tested | Pending | |
| Interface Security | Not Tested | Pending | |
| Network Segmentation | Design Complete | Policy matrix | |
| Firewall Policies | Configuration Prepared | Config files | |
| Firewall Objects | Configuration Prepared | Config files | |
| NAT and VIP | Configuration Prepared | Config files | |
| Antivirus | Not Tested | Pending | |
| IPS | Not Tested | Pending | |
| Web Filtering | Not Tested | Pending | |
| DNS Filtering | Not Tested | Pending | |
| Application Control | Not Tested | Pending | |
| SSL Inspection | Not Tested | Pending | |
| Guest Isolation | Not Tested | Pending | |
| DMZ Isolation | Not Tested | Pending | |
| SD-WAN | Not Tested | Pending | |
| High Availability | Not Tested | Pending | |
| Logging | Not Tested | Pending | |
| Firmware Management | Review Required | Pending | |
| Backup and Recovery | Not Tested | Pending | |
| Repository Sanitization | Review Required | Pending | |

---

# 30. Final Approval

```text
Review Date       :
FortiGate Model   :
FortiOS Version   :
FortiOS Build     :
Reviewed By       :
Technical Result  :
Security Result   :
Outstanding Risks :
Next Review Date  :
```

Select the final result:

```text
[ ] Hardening Validated
[ ] Hardening Validated with Exceptions
[ ] Remediation Required
[ ] Not Yet Tested
```

---

# 31. References

- FortiOS Best Practices
- FortiGate Administration Guide
- FortiOS CLI Reference
- Fortinet PSIRT Advisories
- FortiGate Firmware Upgrade Guide
- Project Firewall Policy Matrix
- Project Implementation Plan

---

# 32. Final Disclaimer

A completed checklist does not guarantee that a firewall is secure.

The actual security posture depends on:

- FortiGate model
- FortiOS build
- Licensing
- Network architecture
- Application requirements
- Threat model
- Operational processes
- Monitoring
- Patch management
- Periodic security review

Every configuration must be independently reviewed and tested before use in
a production environment.
