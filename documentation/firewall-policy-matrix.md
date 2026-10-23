# Enterprise Firewall Policy Matrix

This document defines the proposed firewall policies, security controls,
traffic requirements, inspection profiles, and validation procedures for the
FortiGate Enterprise Security Lab.

The lab represents a fictional enterprise environment protected by a
FortiGate Next-Generation Firewall.

> **Disclaimer**
>
> All company names, domain names, IP addresses, usernames, systems, and
> configurations in this document are fictional or sanitized.
>
> This document was created exclusively for educational and portfolio
> purposes. It does not contain production information, customer data,
> credentials, or confidential configurations.

---

## 1. Document Information

| Item | Value |
|---|---|
| Project | FortiGate Enterprise Security Lab |
| Environment | Fictional enterprise network |
| Document Type | Firewall Policy Matrix |
| Firewall Platform | FortiGate NGFW |
| Configuration Style | FortiOS CLI and GUI concepts |
| Inspection Mode | Flow-based |
| Default Security Model | Explicit allow with implicit deny |
| Author | Elfan Mahrobi |
| Status | Lab Design |
| Classification | Public / Portfolio |

---

## 2. Objectives

The objectives of this firewall policy matrix are to:

- Define approved communication between enterprise security zones
- Apply least-privilege access control
- Prevent unrestricted inter-VLAN communication
- Separate corporate, server, guest, management, DMZ, and security networks
- Protect internet-bound traffic using FortiGate security profiles
- Publish a fictional HTTPS service through a controlled Virtual IP
- Restrict communication from the DMZ to internal systems
- Isolate guest devices from enterprise networks
- Restrict FortiGate administration to a dedicated management workstation
- Forward infrastructure logs to a centralized logging server
- Define logging and inspection requirements for each firewall policy
- Provide test cases for implementation validation

---

## 3. Security Design Principles

The firewall design follows these principles:

### 3.1 Default Deny

Traffic is denied unless an explicit firewall policy allows the required
communication.

### 3.2 Least Privilege

Each policy permits only the required:

- Source interface
- Destination interface
- Source address
- Destination address
- Protocol
- Service port
- Schedule
- Application
- User or system

### 3.3 Network Segmentation

Corporate users, servers, guests, management systems, public services, and
security systems are placed in separate VLANs and security zones.

### 3.4 Specific Policy Objects

Firewall policies should use specific address objects and service objects
instead of broad `all` or `ALL` objects whenever possible.

### 3.5 Controlled Administrative Access

FortiGate management services are available only from the dedicated
Management VLAN and approved administrative workstation.

### 3.6 Security Inspection

Internet-facing and internet-bound policies apply security profiles where
technically appropriate.

### 3.7 Logging and Accountability

Allowed and denied traffic is logged according to policy risk and operational
requirements.

### 3.8 Change Control

Firewall policy changes should include:

- Business justification
- Approval
- Implementation procedure
- Test plan
- Rollback procedure
- Evidence
- Review date
- Policy owner

---

## 4. Logical Security Zones

| Zone | Interface | VLAN | Subnet | Gateway | Purpose |
|---|---|---:|---|---|---|
| CORP-USERS | `VLAN10-USERS` | 10 | `10.10.10.0/24` | `10.10.10.1` | Corporate workstations |
| INTERNAL-SERVERS | `VLAN20-SERVERS` | 20 | `10.10.20.0/24` | `10.10.20.1` | Internal business systems |
| GUEST | `VLAN30-GUEST` | 30 | `10.10.30.0/24` | `10.10.30.1` | Guest internet access |
| MANAGEMENT | `VLAN40-MGMT` | 40 | `10.10.40.0/24` | `10.10.40.1` | Administrative systems |
| DMZ | `VLAN50-DMZ` | 50 | `10.10.50.0/24` | `10.10.50.1` | Public-facing services |
| SECURITY | `VLAN60-SECURITY` | 60 | `10.10.60.0/24` | `10.10.60.1` | Logging and monitoring |
| WAN | `virtual-wan-link` | N/A | N/A | N/A | Internet connectivity |
| WAN1 | `wan1` | N/A | `203.0.113.0/30` | `203.0.113.1` | Primary ISP |
| WAN2 | `wan2` | N/A | `198.51.100.0/30` | `198.51.100.1` | Secondary ISP |

---

## 5. Dummy Systems

| Hostname | Address Object | IP Address | Zone | Function |
|---|---|---|---|---|
| `dc01.example.local` | `HOST-DC01` | `10.10.20.10` | INTERNAL-SERVERS | Active Directory and DNS |
| `app01.example.local` | `HOST-APP01` | `10.10.20.20` | INTERNAL-SERVERS | Internal web application |
| `db01.example.local` | `HOST-DB01` | `10.10.20.30` | INTERNAL-SERVERS | PostgreSQL database |
| `web01.example.local` | `HOST-WEB01` | `10.10.50.10` | DMZ | Public HTTPS application |
| `admin01.example.local` | `HOST-ADMIN01` | `10.10.40.10` | MANAGEMENT | Administrative workstation |
| `log01.example.local` | `HOST-LOG01` | `10.10.60.10` | SECURITY | Central log server |
| `monitor01.example.local` | `HOST-MONITOR01` | `10.10.60.20` | SECURITY | Network monitoring server |
| `ntp01.example.local` | `HOST-NTP01` | `10.10.20.40` | INTERNAL-SERVERS | Internal NTP server |

---

## 6. Address Objects

| Object Name | Address | Type | Description |
|---|---|---|---|
| `NET-CORP-USERS` | `10.10.10.0/24` | Subnet | Corporate user network |
| `NET-INTERNAL-SERVERS` | `10.10.20.0/24` | Subnet | Internal server network |
| `NET-GUEST` | `10.10.30.0/24` | Subnet | Guest network |
| `NET-MANAGEMENT` | `10.10.40.0/24` | Subnet | Management network |
| `NET-DMZ` | `10.10.50.0/24` | Subnet | DMZ network |
| `NET-SECURITY` | `10.10.60.0/24` | Subnet | Security network |
| `HOST-DC01` | `10.10.20.10/32` | Host | Active Directory and DNS |
| `HOST-APP01` | `10.10.20.20/32` | Host | Internal application |
| `HOST-DB01` | `10.10.20.30/32` | Host | Database server |
| `HOST-NTP01` | `10.10.20.40/32` | Host | Internal NTP server |
| `HOST-WEB01` | `10.10.50.10/32` | Host | DMZ web server |
| `HOST-ADMIN01` | `10.10.40.10/32` | Host | Administrative workstation |
| `HOST-LOG01` | `10.10.60.10/32` | Host | Central logging server |
| `HOST-MONITOR01` | `10.10.60.20/32` | Host | Monitoring server |
| `RFC1918-10` | `10.0.0.0/8` | Subnet | Private address range |
| `RFC1918-172` | `172.16.0.0/12` | Subnet | Private address range |
| `RFC1918-192` | `192.168.0.0/16` | Subnet | Private address range |
| `GRP-RFC1918` | RFC1918 objects | Group | All private IPv4 networks |

---

## 7. Custom Service Objects

| Service Name | Protocol | Destination Port | Purpose |
|---|---|---:|---|
| `HTTPS-443` | TCP | 443 | Secure web access |
| `HTTP-80` | TCP | 80 | Web access |
| `DNS-UDP` | UDP | 53 | DNS query |
| `DNS-TCP` | TCP | 53 | DNS query and large response |
| `LDAP-389` | TCP/UDP | 389 | LDAP |
| `LDAPS-636` | TCP | 636 | Secure LDAP |
| `KERBEROS-TCP` | TCP | 88 | Kerberos authentication |
| `KERBEROS-UDP` | UDP | 88 | Kerberos authentication |
| `NTP-UDP` | UDP | 123 | Time synchronization |
| `POSTGRESQL-5432` | TCP | 5432 | PostgreSQL database |
| `SYSLOG-TLS-6514` | TCP | 6514 | Encrypted syslog |
| `SNMP-UDP-161` | UDP | 161 | SNMP monitoring |
| `ICMP-PING` | ICMP | N/A | Connectivity testing |
| `SSH-22` | TCP | 22 | Secure administration |

---

## 8. Service Groups

### `SVC-INTERNET-STANDARD`

Contains:

- `DNS-UDP`
- `DNS-TCP`
- `HTTP-80`
- `HTTPS-443`

### `SVC-ACTIVE-DIRECTORY`

Contains:

- DNS
- Kerberos
- LDAP
- LDAPS

Additional Active Directory services should be added only when required and
properly validated.

### `SVC-FORTIGATE-MANAGEMENT`

Contains:

- HTTPS
- SSH
- PING

### `SVC-SECURE-LOGGING`

Contains:

- `SYSLOG-TLS-6514`

---

## 9. Security Profile Assignment

| Profile Type | Profile Name | Inspection Mode | Primary Purpose |
|---|---|---|---|
| Antivirus | `AV-CORPORATE` | Flow-based | Malware prevention |
| IPS | `IPS-CORPORATE` | Flow-based | Client and server exploit prevention |
| IPS | `IPS-PROTECT-SERVER` | Flow-based | Internet-facing server protection |
| Web Filter | `WEB-CORPORATE` | Flow-based | Corporate web category control |
| Web Filter | `WEB-GUEST` | Flow-based | Restricted guest internet access |
| DNS Filter | `DNS-CORPORATE` | Flow-based | Malicious-domain blocking |
| DNS Filter | `DNS-GUEST` | Flow-based | Guest DNS protection |
| Application Control | `APP-CORPORATE` | Flow-based | Application identification and control |
| Application Control | `APP-GUEST` | Flow-based | Restrict guest applications |
| SSL Inspection | `certificate-inspection` | Certificate | Inspect TLS certificate information |

---

## 10. Firewall Policy Summary

| ID | Policy Name | Source | Destination | Services | Action | NAT | Security Profiles | Logging |
|---:|---|---|---|---|---|---|---|---|
| 10 | `CORP-USERS-to-INTERNET` | Corporate users | Internet | DNS, HTTP, HTTPS | Allow | Source NAT | Full corporate profiles | All sessions |
| 20 | `CORP-USERS-to-APP01` | Corporate users | Internal application | HTTPS | Allow | Disabled | IPS | All sessions |
| 21 | `CORP-USERS-to-DC01-DNS` | Corporate users | DNS server | DNS | Allow | Disabled | IPS | All sessions |
| 22 | `CORP-USERS-to-DC01-AUTH` | Corporate users | Domain controller | Kerberos, LDAP, LDAPS | Allow | Disabled | IPS | All sessions |
| 23 | `CORP-USERS-to-NTP01` | Corporate users | NTP server | NTP | Allow | Disabled | None | All sessions |
| 24 | `CORP-USERS-to-DB01-DENY` | Corporate users | Database server | All | Deny | Disabled | None | All sessions |
| 30 | `GUEST-to-INTERNET` | Guest clients | Internet | DNS, HTTP, HTTPS | Allow | Source NAT | Guest profiles | All sessions |
| 31 | `GUEST-to-RFC1918-DENY` | Guest clients | Private networks | All | Deny | Disabled | None | All sessions |
| 40 | `INTERNET-to-DMZ-PORTAL` | Internet | Public HTTPS VIP | HTTPS | Allow | VIP/DNAT | Server IPS | All sessions |
| 41 | `INTERNET-to-DMZ-ADMIN-DENY` | Internet | DMZ servers | SSH, RDP | Deny | Disabled | None | All sessions |
| 50 | `WEB01-to-DB01` | DMZ web server | Database server | PostgreSQL | Allow | Disabled | IPS | All sessions |
| 51 | `WEB01-to-DNS` | DMZ web server | DNS server | DNS | Allow | Disabled | IPS | All sessions |
| 52 | `WEB01-to-NTP` | DMZ web server | NTP server | NTP | Allow | Disabled | None | All sessions |
| 53 | `DMZ-to-INTERNAL-DENY` | DMZ network | Internal servers | All | Deny | Disabled | None | All sessions |
| 60 | `FORTIGATE-to-LOG01` | FortiGate | Log server | Syslog TLS | Allow/local | Disabled | None | System logs |
| 61 | `DEVICES-to-LOG01` | Approved devices | Log server | Syslog TLS | Allow | Disabled | None | All sessions |
| 62 | `MONITOR01-to-NETWORK` | Monitoring server | Approved devices | SNMP, ICMP | Allow | Disabled | None | All sessions |
| 70 | `ADMIN01-to-FORTIGATE` | Admin workstation | FortiGate | HTTPS, SSH, PING | Allow/local | Disabled | None | Admin events |
| 71 | `ADMIN01-to-SERVERS` | Admin workstation | Approved servers | HTTPS, SSH | Allow | Disabled | IPS | All sessions |
| 72 | `NON-MGMT-to-FORTIGATE-DENY` | Non-management networks | FortiGate | Admin services | Deny/local | Disabled | None | All sessions |
| 90 | `INTERZONE-DEFAULT-DENY` | Internal zones | Other zones | All | Deny | Disabled | None | All sessions |

> FortiGate administrative access to the firewall itself is primarily
> controlled through interface administrative-access settings, administrator
> trusted hosts, local-in policy, and administrator permissions. It is not
> handled exactly like ordinary forwarded traffic.

---

# 11. Detailed Firewall Policies

## Policy 10: Corporate Users to Internet

### Purpose

Allow corporate users to access approved internet services while applying
security inspection and source NAT.

### Policy Definition

```text
Policy ID             : 10
Policy name           : CORP-USERS-to-INTERNET
Source interface      : VLAN10-USERS
Destination interface : virtual-wan-link
Source address        : NET-CORP-USERS
Destination address   : all
Schedule              : always
Services              : SVC-INTERNET-STANDARD
Action                : Accept
Source NAT            : Enabled
Inspection mode       : Flow-based
Logging               : All sessions
```

### Security Profiles

Policy 10 applies multiple security profiles to inspect corporate user traffic
before the traffic reaches the internet.

#### Antivirus

```text
Profile name    : AV-CORPORATE
Inspection mode : Flow-based
Action          : Block
Logging         : Enabled
```

The antivirus profile scans supported traffic and blocks files matching known
malware signatures.

#### Intrusion Prevention System

```text
Profile name    : IPS-CORPORATE
Inspection mode : Flow-based
Target          : Client and Server
Severity        : Critical, High, and Medium
Action          : Block
Logging         : Enabled
Packet logging  : Critical events
```

The IPS sensor is intended to detect and block:

- Client-side exploits
- Browser exploits
- Protocol anomalies
- Known network attacks
- Suspicious application traffic
- Exploit attempts against supported services

The IPS sensor should be tuned based on the systems and applications present
in the environment.

#### Web Filtering

```text
Profile name : WEB-CORPORATE
Action       : Block high-risk categories
Logging      : Enabled
```

Example blocked categories:

- Malicious websites
- Phishing
- Spam URLs
- Botnet and command-and-control services
- Proxy avoidance
- Peer-to-peer file sharing
- Cryptocurrency mining
- Newly registered or newly observed domains

Business-related categories remain allowed or monitored according to the
fictional organization's acceptable-use policy.

#### DNS Filtering

```text
Profile name        : DNS-CORPORATE
FortiGuard rating   : Enabled
Botnet C&C blocking : Enabled
Logging             : Enabled
```

The DNS-filter profile protects users from:

- Phishing domains
- Malware-distribution domains
- Botnet command-and-control domains
- Suspicious domains
- Locally blocked domains

DNS filtering may block a malicious domain before an HTTP or HTTPS session is
established.

#### Application Control

```text
Profile name : APP-CORPORATE
Logging      : Enabled
```

Example blocked or restricted applications:

- Peer-to-peer applications
- Unauthorized remote-access tools
- Public proxy applications
- Anonymizer services
- Cryptocurrency-mining applications
- High-risk file-sharing applications
- Applications categorized as malicious

Approved business applications remain allowed and logged.

#### SSL Inspection

```text
Profile name : certificate-inspection
Mode         : Certificate inspection
```

Certificate inspection is used as the baseline because it does not decrypt the
complete HTTPS payload.

Deep SSL inspection requires:

- FortiGate CA certificate deployment
- Endpoint trust configuration
- Privacy and legal review
- Application compatibility testing
- Certificate-pinning exceptions
- Performance assessment
- Formal change approval

#### Logging Requirements

Policy 10 records:

- Allowed sessions
- Blocked sessions
- Antivirus detections
- IPS detections
- Web-filter violations
- DNS-filter violations
- Application-control events
- SSL certificate anomalies

### Validation

| Test | Expected Result |
|---|---|
| User accesses an approved HTTPS site | Allowed and logged |
| User accesses phishing test category | Blocked and logged |
| User downloads a known safe test file | Allowed |
| User triggers approved antivirus test | Blocked and logged |
| User uses unauthorized proxy application | Blocked or monitored |
| User accesses internet using unauthorized port | Blocked |
| Primary ISP is unavailable | Traffic follows approved backup path |

---

## Policy 20: Corporate Users to Internal Application

### Purpose

Allow corporate users to access the internal business application over HTTPS.

### Policy Definition

```text
Policy ID             : 20
Policy name           : CORP-USERS-to-APP01
Source interface      : VLAN10-USERS
Destination interface : VLAN20-SERVERS
Source address        : NET-CORP-USERS
Destination address   : HOST-APP01
Schedule              : always
Service               : HTTPS-443
Action                : Accept
NAT                    : Disabled
IPS profile            : IPS-CORPORATE
Logging                : All sessions
```

### Security Requirements

- Only HTTPS is allowed
- Direct HTTP access is not included
- Access to other server addresses is not implied
- IPS inspection is applied
- All sessions are logged
- Source NAT is disabled to preserve the original client address

### Validation

| Test | Expected Result |
|---|---|
| Corporate user accesses `app01` using HTTPS | Allowed |
| Corporate user accesses `app01` using SSH | Blocked |
| Corporate user accesses another unapproved server | Blocked |
| Application access log records original client IP | Successful |

---

## Policy 21: Corporate Users to DNS Server

### Purpose

Allow corporate users to resolve enterprise and internet domain names using
the approved DNS server.

### Policy Definition

```text
Policy ID             : 21
Policy name           : CORP-USERS-to-DC01-DNS
Source interface      : VLAN10-USERS
Destination interface : VLAN20-SERVERS
Source address        : NET-CORP-USERS
Destination address   : HOST-DC01
Schedule              : always
Services              : DNS-UDP, DNS-TCP
Action                : Accept
NAT                    : Disabled
IPS profile            : IPS-CORPORATE
Logging                : All sessions
```

### Security Requirements

- DNS queries are sent only to the approved internal DNS server
- Direct DNS access to unauthorized external resolvers may be restricted
- TCP/53 is included for large DNS responses and supported DNS operations
- DNS events are logged

### Validation

| Test | Expected Result |
|---|---|
| Client queries `dc01` over UDP/53 | Allowed |
| Client queries `dc01` over TCP/53 | Allowed |
| Client queries unauthorized internal DNS server | Blocked |
| Client attempts unauthorized DNS protocol | Blocked |

---

## Policy 22: Corporate Users to Identity Services

### Purpose

Allow corporate users to access approved identity and authentication services
on the domain controller.

### Policy Definition

```text
Policy ID             : 22
Policy name           : CORP-USERS-to-DC01-AUTH
Source interface      : VLAN10-USERS
Destination interface : VLAN20-SERVERS
Source address        : NET-CORP-USERS
Destination address   : HOST-DC01
Schedule              : always
Services              : SVC-ACTIVE-DIRECTORY
Action                : Accept
NAT                    : Disabled
IPS profile            : IPS-CORPORATE
Logging                : All sessions
```

### Security Requirements

- Only documented identity services are permitted
- Service requirements must be validated against the lab use case
- Unnecessary Active Directory-related ports must not be added
- LDAP should be replaced with LDAPS where supported

### Validation

| Test | Expected Result |
|---|---|
| Corporate client performs approved authentication | Allowed |
| Corporate client uses unauthorized service to `dc01` | Blocked |
| Corporate client accesses unrelated server service | Blocked |

---

## Policy 23: Corporate Users to Internal NTP Server

### Purpose

Allow corporate endpoints to synchronize time with the approved internal NTP
server.

### Policy Definition

```text
Policy ID             : 23
Policy name           : CORP-USERS-to-NTP01
Source interface      : VLAN10-USERS
Destination interface : VLAN20-SERVERS
Source address        : NET-CORP-USERS
Destination address   : HOST-NTP01
Schedule              : always
Service               : NTP-UDP
Action                : Accept
NAT                    : Disabled
Logging                : All sessions
```

### Validation

| Test | Expected Result |
|---|---|
| Corporate client synchronizes with `ntp01` | Allowed |
| Corporate client connects to another service on `ntp01` | Blocked |

---

## Policy 24: Corporate Users to Database Deny

### Purpose

Explicitly prevent corporate workstations from accessing the database server
directly.

### Policy Definition

```text
Policy ID             : 24
Policy name           : CORP-USERS-to-DB01-DENY
Source interface      : VLAN10-USERS
Destination interface : VLAN20-SERVERS
Source address        : NET-CORP-USERS
Destination address   : HOST-DB01
Schedule              : always
Service               : ALL
Action                : Deny
NAT                    : Disabled
Logging                : All sessions
```

### Security Rationale

Only an approved application server should communicate with the database.
Direct access from user workstations increases the attack surface and bypasses
application-layer authorization controls.

### Validation

| Test | Expected Result |
|---|---|
| User connects to `db01` on TCP/5432 | Blocked and logged |
| User pings `db01` | Blocked unless explicitly approved |
| Application server connects to `db01` | Evaluated by separate policy |

---

## Policy 30: Guest Network to Internet

### Purpose

Allow guest devices to access standard internet services without access to
enterprise resources.

### Policy Definition

```text
Policy ID             : 30
Policy name           : GUEST-to-INTERNET
Source interface      : VLAN30-GUEST
Destination interface : virtual-wan-link
Source address        : NET-GUEST
Destination address   : all
Schedule              : always
Services              : SVC-INTERNET-STANDARD
Action                : Accept
Source NAT            : Enabled
Inspection mode       : Flow-based
Logging               : All sessions
```

### Security Profiles

```text
SSL inspection      : certificate-inspection
Web filter          : WEB-GUEST
DNS filter          : DNS-GUEST
Application control : APP-GUEST
Antivirus           : AV-CORPORATE
IPS                 : IPS-CORPORATE
```

### Guest Restrictions

The following traffic is prohibited:

- Guest to corporate users
- Guest to internal servers
- Guest to Management VLAN
- Guest to Security VLAN
- Guest to DMZ private addresses
- Guest to FortiGate management services
- Guest to unauthorized protocols
- Guest to known malicious destinations

### Validation

| Test | Expected Result |
|---|---|
| Guest accesses approved HTTPS site | Allowed and logged |
| Guest accesses internal application | Blocked |
| Guest accesses FortiGate HTTPS management | Blocked |
| Guest accesses malicious domain | Blocked and logged |
| Guest starts peer-to-peer application | Blocked or restricted |

---

## Policy 31: Guest to Private Networks Deny

### Purpose

Explicitly deny guest access to RFC1918 private networks.

### Policy Definition

```text
Policy ID             : 31
Policy name           : GUEST-to-RFC1918-DENY
Source interface      : VLAN30-GUEST
Destination interface : Internal interfaces or zones
Source address        : NET-GUEST
Destination address   : GRP-RFC1918
Schedule              : always
Service               : ALL
Action                : Deny
NAT                    : Disabled
Logging                : All sessions
```

### Policy Placement

This deny policy must be placed before broad rules that might otherwise allow
guest traffic.

### Validation

| Test | Expected Result |
|---|---|
| Guest accesses `10.10.10.10` | Blocked |
| Guest accesses `10.10.20.20` | Blocked |
| Guest accesses `10.10.40.1` | Blocked |
| Guest accesses public internet address | Evaluated by Policy 30 |

---

## Policy 40: Internet to DMZ Portal

### Purpose

Publish the fictional public web application through HTTPS.

### Virtual IP Definition

```text
VIP name        : VIP-PORTAL-HTTPS
External IP     : 203.0.113.10
External port   : TCP/443
Mapped IP       : 10.10.50.10
Mapped port     : TCP/443
External service: https://portal.example.com
```

### Policy Definition

```text
Policy ID             : 40
Policy name           : INTERNET-to-DMZ-PORTAL
Source interface      : wan1
Destination interface : VLAN50-DMZ
Source address        : all
Destination address   : VIP-PORTAL-HTTPS
Schedule              : always
Service               : HTTPS-443
Action                : Accept
NAT                    : Disabled
Destination NAT       : VIP
IPS profile            : IPS-PROTECT-SERVER
SSL inspection         : certificate-inspection
Logging                : All sessions
```

### Security Requirements

- Only TCP/443 is published
- SSH is not exposed
- RDP is not exposed
- HTTP is not exposed unless used only for controlled HTTPS redirection
- Server-focused IPS protection is enabled
- All inbound sessions are logged
- A valid TLS certificate should be installed on the appropriate termination
  point
- The application should be protected by a WAF in an expanded design
- The application should undergo vulnerability assessment and penetration
  testing

### Validation

| Test | Expected Result |
|---|---|
| Internet client connects to public TCP/443 | Allowed and translated |
| Internet client connects to public TCP/80 | Blocked unless approved |
| Internet client connects to TCP/22 | Blocked |
| Internet client connects directly to private IP | Not routable |
| IPS test signature is detected | Blocked and logged |

---

## Policy 41: Internet to DMZ Administrative Services Deny

### Purpose

Explicitly block administrative access from the internet to DMZ servers.

### Policy Definition

```text
Policy ID             : 41
Policy name           : INTERNET-to-DMZ-ADMIN-DENY
Source interface      : WAN interfaces
Destination interface : VLAN50-DMZ
Source address        : all
Destination address   : DMZ VIP or published addresses
Schedule              : always
Services              : SSH, RDP
Action                : Deny
NAT                    : Disabled
Logging                : All sessions
```

### Validation

| Test | Expected Result |
|---|---|
| Internet client attempts SSH to public IP | Blocked and logged |
| Internet client attempts RDP to public IP | Blocked and logged |

---

## Policy 50: DMZ Web Server to Database Server

### Purpose

Allow the public web application to access only the required database service.

### Policy Definition

```text
Policy ID             : 50
Policy name           : WEB01-to-DB01
Source interface      : VLAN50-DMZ
Destination interface : VLAN20-SERVERS
Source address        : HOST-WEB01
Destination address   : HOST-DB01
Schedule              : always
Service               : POSTGRESQL-5432
Action                : Accept
NAT                    : Disabled
IPS profile            : IPS-PROTECT-SERVER
Logging                : All sessions
```

### Security Requirements

- Source is restricted to `HOST-WEB01`
- Destination is restricted to `HOST-DB01`
- Service is restricted to TCP/5432
- The `ALL` service is not permitted
- Source NAT is disabled
- IPS inspection is applied
- Database authentication and encryption remain required
- Database access logs should be enabled

### Validation

| Test | Expected Result |
|---|---|
| `web01` connects to `db01` on TCP/5432 | Allowed |
| `web01` connects to `db01` on TCP/22 | Blocked |
| Another DMZ host connects to `db01` | Blocked |
| `web01` connects to another internal server | Blocked |

---

## Policy 51: DMZ Web Server to Internal DNS

### Purpose

Allow the DMZ web server to resolve required domain names using the approved
internal DNS server.

### Policy Definition

```text
Policy ID             : 51
Policy name           : WEB01-to-DNS
Source interface      : VLAN50-DMZ
Destination interface : VLAN20-SERVERS
Source address        : HOST-WEB01
Destination address   : HOST-DC01
Schedule              : always
Services              : DNS-UDP, DNS-TCP
Action                : Accept
NAT                    : Disabled
IPS profile            : IPS-PROTECT-SERVER
Logging                : All sessions
```

### Security Consideration

In a more mature architecture, the DMZ should use a dedicated DNS resolver
instead of communicating directly with a domain controller.

### Validation

| Test | Expected Result |
|---|---|
| `web01` sends approved DNS request | Allowed |
| Other DMZ system sends DNS request | Blocked unless approved |
| `web01` accesses LDAP on `dc01` | Blocked |

---

## Policy 52: DMZ Web Server to Internal NTP

### Purpose

Allow the DMZ web server to synchronize time with the approved NTP server.

### Policy Definition

```text
Policy ID             : 52
Policy name           : WEB01-to-NTP
Source interface      : VLAN50-DMZ
Destination interface : VLAN20-SERVERS
Source address        : HOST-WEB01
Destination address   : HOST-NTP01
Schedule              : always
Service               : NTP-UDP
Action                : Accept
NAT                    : Disabled
Logging                : All sessions
```

### Validation

| Test | Expected Result |
|---|---|
| `web01` synchronizes with `ntp01` | Allowed |
| `web01` accesses another service on `ntp01` | Blocked |

---

## Policy 53: DMZ to Internal Servers Deny

### Purpose

Deny all DMZ-to-internal traffic not explicitly approved by a preceding rule.

### Policy Definition

```text
Policy ID             : 53
Policy name           : DMZ-to-INTERNAL-DENY
Source interface      : VLAN50-DMZ
Destination interface : VLAN20-SERVERS
Source address        : NET-DMZ
Destination address   : NET-INTERNAL-SERVERS
Schedule              : always
Service               : ALL
Action                : Deny
NAT                    : Disabled
Logging                : All sessions
```

### Validation

| Test | Expected Result |
|---|---|
| Unapproved DMZ host accesses internal server | Blocked |
| `web01` accesses approved database port | Matched by Policy 50 |
| `web01` accesses unapproved internal port | Blocked by Policy 53 |

---

## Policy 60: FortiGate to Central Log Server

### Purpose

Forward FortiGate traffic, security, system, and administrative logs to the
central log server.

### Log Destination

```text
Server   : log01.example.local
Address  : 10.10.60.10
Protocol : Syslog over TLS
Port     : TCP/6514
```

### Required Log Types

- Traffic logs
- Security-profile events
- Antivirus events
- IPS events
- Web-filter events
- DNS-filter events
- Application-control events
- Administrator login events
- Configuration changes
- HA events
- Interface events
- Routing events
- System events

### Important Note

Traffic generated directly by FortiGate may require system-level logging or
local-out routing configuration rather than a standard forwarded firewall
policy.

### Validation

| Test | Expected Result |
|---|---|
| FortiGate generates system event | Event received by `log01` |
| Administrator changes configuration | Event received by `log01` |
| IPS blocks traffic | Security event received by `log01` |
| HA failover occurs | HA event received by `log01` |

---

## Policy 61: Approved Devices to Central Log Server

### Purpose

Allow approved infrastructure devices to send encrypted syslog to the central
logging server.

### Policy Definition

```text
Policy ID             : 61
Policy name           : DEVICES-to-LOG01
Source interface      : Approved internal interfaces
Destination interface : VLAN60-SECURITY
Source address        : Approved infrastructure objects
Destination address   : HOST-LOG01
Schedule              : always
Service               : SYSLOG-TLS-6514
Action                : Accept
NAT                    : Disabled
Logging                : All sessions
```

### Validation

| Test | Expected Result |
|---|---|
| Approved device sends TCP/6514 to `log01` | Allowed |
| User workstation sends syslog to `log01` | Blocked |
| Approved device accesses another service on `log01` | Blocked |

---

## Policy 62: Monitoring Server to Approved Devices

### Purpose

Allow the monitoring server to perform approved availability and health
checks.

### Policy Definition

```text
Policy ID             : 62
Policy name           : MONITOR01-to-NETWORK
Source interface      : VLAN60-SECURITY
Destination interface : Approved internal interfaces
Source address        : HOST-MONITOR01
Destination address   : Approved network-device objects
Schedule              : always
Services              : SNMP-UDP-161, ICMP-PING
Action                : Accept
NAT                    : Disabled
Logging                : All sessions
```

### Security Requirements

- SNMPv3 should be used where supported
- Default SNMP community strings must not be used
- Destination devices must be explicitly defined
- Monitoring traffic must not use unrestricted service access
- Administrative protocols are excluded from this policy

### Validation

| Test | Expected Result |
|---|---|
| `monitor01` polls approved device over SNMP | Allowed |
| `monitor01` pings approved device | Allowed |
| `monitor01` accesses device over SSH | Blocked unless separately approved |
| Another security host performs SNMP polling | Blocked |

---

## Policy 70: Administrative Workstation to FortiGate

### Purpose

Restrict FortiGate administration to the approved workstation in the
Management VLAN.

### Access Definition

```text
Source interface : VLAN40-MGMT
Source address   : HOST-ADMIN01
Destination      : FortiGate management interface
Services         : HTTPS, SSH, PING
Action           : Allow
Logging          : Administrative and local traffic events
```

### Required Controls

- Enable HTTPS, SSH, and PING only on the Management interface
- Disable HTTP and Telnet
- Do not enable administrative access on WAN interfaces
- Configure administrator trusted hosts
- Use individual administrator accounts
- Enable multi-factor authentication where available
- Apply least-privilege administrator profiles
- Configure administrative session timeout
- Log all administrator activity
- Protect management access with a local-in policy where appropriate

### Validation

| Test | Expected Result |
|---|---|
| `admin01` accesses FortiGate HTTPS | Allowed |
| `admin01` accesses FortiGate SSH | Allowed |
| Corporate user accesses FortiGate HTTPS | Blocked |
| Guest accesses FortiGate HTTPS | Blocked |
| Internet client accesses FortiGate administration | Blocked |

---

## Policy 71: Administrative Workstation to Servers

### Purpose

Allow the approved administrative workstation to manage explicitly approved
servers.

### Policy Definition

```text
Policy ID             : 71
Policy name           : ADMIN01-to-SERVERS
Source interface      : VLAN40-MGMT
Destination interface : VLAN20-SERVERS or VLAN50-DMZ
Source address        : HOST-ADMIN01
Destination address   : Approved server objects
Schedule              : Approved maintenance schedule
Services              : HTTPS, SSH
Action                : Accept
NAT                    : Disabled
IPS profile            : IPS-CORPORATE
Logging                : All sessions
```

### Security Requirements

- Destination servers must be explicitly listed
- RDP should be added only when required
- Direct database administration requires separate approval
- Access should preferably use a privileged access workstation or jump server
- All administrative sessions must be logged
- Temporary policies should have expiration dates

### Validation

| Test | Expected Result |
|---|---|
| `admin01` accesses approved server over SSH | Allowed |
| `admin01` accesses unapproved server | Blocked |
| Corporate workstation accesses server administration | Blocked |

---

## Policy 72: Non-Management Access to FortiGate Deny

### Purpose

Prevent administrative access to FortiGate from unauthorized network zones.

### Control Definition

```text
Source zones : CORP-USERS, GUEST, INTERNAL-SERVERS, DMZ
Destination  : FortiGate interface addresses
Services     : HTTPS, HTTP, SSH, TELNET, SNMP
Action       : Deny
Logging      : Enabled
```

### Enforcement Mechanisms

This requirement should be enforced using:

- Interface administrative-access settings
- Administrator trusted hosts
- Local-in policies
- Dedicated Management VLAN
- Strong administrator authentication

### Validation

| Test | Expected Result |
|---|---|
| User workstation accesses FortiGate HTTPS | Blocked |
| Guest device accesses FortiGate SSH | Blocked |
| DMZ server accesses FortiGate HTTPS | Blocked |
| `admin01` accesses Management interface | Allowed |

---

## Policy 90: Inter-Zone Default Deny

### Purpose

Deny any inter-zone traffic that is not explicitly permitted.

### Policy Definition

```text
Policy ID             : 90
Policy name           : INTERZONE-DEFAULT-DENY
Source interface      : Internal zones
Destination interface : Other internal zones
Source address        : all
Destination address   : all
Schedule              : always
Service               : ALL
Action                : Deny
NAT                    : Disabled
Logging                : All sessions
```

### Security Requirement

This rule is used for visibility and explicit denied-traffic logging. FortiGate
also provides an implicit deny at the end of the policy list.

### Validation

| Test | Expected Result |
|---|---|
| Unapproved source communicates across zones | Blocked and logged |
| Approved traffic matches a preceding allow policy | Allowed |
| Denied traffic appears in logs | Successful |

---

# 12. Recommended Policy Order

Policies should be ordered from most specific to most general.

Recommended high-level order:

1. Specific management policies
2. Specific application-access policies
3. Specific identity and infrastructure policies
4. Specific user-to-server deny policies
5. Guest private-network deny policies
6. Internet-to-DMZ publishing policies
7. DMZ-to-internal allow policies
8. DMZ-to-internal deny policies
9. Security logging and monitoring policies
10. Corporate internet-access policies
11. Guest internet-access policies
12. Explicit inter-zone deny policies
13. FortiGate implicit deny

The actual FortiGate policy placement must also consider interface direction.
Policies operating on different interface pairs are evaluated within their
applicable traffic path.

---

# 13. Logging Requirements

## 13.1 Allowed Traffic

Log allowed traffic for:

- Internet access
- Internal application access
- Identity-service access
- DMZ application publishing
- DMZ-to-database access
- Management access
- Logging traffic
- Monitoring traffic

## 13.2 Denied Traffic

Log denied traffic for:

- Guest-to-private access
- User-to-database access
- DMZ-to-internal violations
- Unauthorized administrative access
- Internet-to-administrative services
- Inter-zone policy violations

## 13.3 Security Events

Collect:

- Antivirus detections
- IPS detections
- Web-filter blocks
- DNS-filter blocks
- Application-control blocks
- Botnet detections
- SSL certificate anomalies
- Administrator login failures
- Configuration changes
- HA state changes

## 13.4 Log Protection

- Use encrypted transport where supported
- Restrict access to the log server
- Synchronize time across all systems
- Define log-retention requirements
- Prevent unauthorized log deletion
- Monitor log-forwarding failures
- Back up critical security logs

---

# 14. Firewall Policy Naming Standard

Recommended format:

```text
<SOURCE>-to-<DESTINATION>-<PURPOSE>
```

Examples:

```text
CORP-USERS-to-INTERNET
CORP-USERS-to-APP01
GUEST-to-RFC1918-DENY
INTERNET-to-DMZ-PORTAL
WEB01-to-DB01
ADMIN01-to-SERVERS
```

Policy names should be:

- Unique
- Descriptive
- Consistent
- Easy to identify in logs
- Independent of temporary ticket numbers

Ticket or change references may be added in the policy comments.

---

# 15. Required Policy Comments

Each implemented policy should include comments containing:

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

Example:

```text
Purpose: Allow corporate users to access the internal HTTPS application
Business owner: Application Owner
Technical owner: Security Engineering
Change reference: LAB-CHG-001
Implementation date: YYYY-MM-DD
Review date: YYYY-MM-DD
Expiration date: Permanent
Risk classification: Medium
```

---

# 16. Change-Control Requirements

Before creating or modifying a firewall policy, record:

- Requestor
- Business justification
- Application owner
- Technical owner
- Source networks
- Destination systems
- Required ports and protocols
- Data classification
- Security inspection requirement
- Logging requirement
- Implementation procedure
- Test procedure
- Rollback procedure
- Maintenance window
- Security approval
- Policy review date
- Expiration date for temporary access

Temporary policies must include:

- Named owner
- Business justification
- Expiration date
- Review requirement
- Removal evidence

---

# 17. Implementation Checklist

## Pre-Implementation

- [ ] Business requirement has been documented
- [ ] Source interface has been confirmed
- [ ] Destination interface has been confirmed
- [ ] Source address has been validated
- [ ] Destination address has been validated
- [ ] Required ports have been confirmed
- [ ] Address objects have been created
- [ ] Service objects have been created
- [ ] Security profiles have been created
- [ ] NAT requirements have been confirmed
- [ ] Routing has been validated
- [ ] DNS requirements have been validated
- [ ] Logging destination is available
- [ ] Existing overlapping policies have been reviewed
- [ ] Policy order has been reviewed
- [ ] Configuration backup has been completed
- [ ] Rollback procedure has been prepared
- [ ] Change approval has been obtained

## Implementation

- [ ] Policy name follows the naming standard
- [ ] Source interface is specific
- [ ] Destination interface is specific
- [ ] Source address is restricted
- [ ] Destination address is restricted
- [ ] Services are restricted
- [ ] Schedule is appropriate
- [ ] Action is correct
- [ ] NAT setting is correct
- [ ] Security profiles are assigned
- [ ] SSL inspection mode is correct
- [ ] Logging is enabled
- [ ] Policy comments are complete
- [ ] Policy order is correct
- [ ] Configuration has been saved

## Post-Implementation

- [ ] Allowed traffic has been tested
- [ ] Unauthorized traffic has been tested
- [ ] NAT behavior has been verified
- [ ] Security-profile inspection has been verified
- [ ] Traffic logs have been verified
- [ ] Security logs have been verified
- [ ] Source IP visibility has been verified
- [ ] Application functionality has been confirmed
- [ ] Performance has been observed
- [ ] HA synchronization has been confirmed
- [ ] Monitoring has been confirmed
- [ ] Test evidence has been collected
- [ ] Change record has been updated
- [ ] Configuration backup has been completed

---

# 18. Rollback Plan

If implementation causes unexpected service impact:

1. Disable the newly created firewall policy
2. Restore the previous policy state
3. Remove or disable new Virtual IP objects if applicable
4. Restore the previous route or SD-WAN configuration if changed
5. Verify application connectivity
6. Verify internet connectivity
7. Verify inter-zone access
8. Confirm logging and monitoring status
9. Notify the application owner
10. Document the failure and collected evidence
11. Restore the approved configuration backup if required

Rollback should avoid deleting evidence before troubleshooting is complete.

---

# 19. Validation Test Matrix

| Test ID | Scenario | Source | Destination | Expected Result |
|---|---|---|---|---|
| TC-001 | Corporate HTTPS internet access | Corporate user | Approved website | Allowed and logged |
| TC-002 | Corporate malicious website access | Corporate user | Malicious test category | Blocked and logged |
| TC-003 | Corporate unauthorized outbound port | Corporate user | Internet | Blocked |
| TC-004 | Corporate access to internal application | Corporate user | `app01:443` | Allowed |
| TC-005 | Corporate SSH access to application | Corporate user | `app01:22` | Blocked |
| TC-006 | Corporate DNS request | Corporate user | `dc01:53` | Allowed |
| TC-007 | Corporate direct database access | Corporate user | `db01:5432` | Blocked |
| TC-008 | Guest internet access | Guest client | Approved website | Allowed |
| TC-009 | Guest access to internal application | Guest client | `app01:443` | Blocked |
| TC-010 | Guest access to FortiGate | Guest client | FortiGate HTTPS | Blocked |
| TC-011 | Internet access to public portal | Internet client | Public TCP/443 | Allowed through VIP |
| TC-012 | Internet SSH to public IP | Internet client | Public TCP/22 | Blocked |
| TC-013 | DMZ web server database access | `web01` | `db01:5432` | Allowed |
| TC-014 | DMZ web server SSH to database | `web01` | `db01:22` | Blocked |
| TC-015 | Unapproved DMZ-to-internal access | DMZ host | Internal server | Blocked |
| TC-016 | Administrator FortiGate HTTPS access | `admin01` | FortiGate HTTPS | Allowed |
| TC-017 | Corporate user FortiGate HTTPS access | Corporate user | FortiGate HTTPS | Blocked |
| TC-018 | Approved syslog forwarding | Network device | `log01:6514` | Allowed |
| TC-019 | Unauthorized syslog source | User workstation | `log01:6514` | Blocked |
| TC-020 | Monitoring SNMP request | `monitor01` | Approved device | Allowed |
| TC-021 | Unauthorized SNMP request | Corporate user | Network device | Blocked |
| TC-022 | Primary WAN interruption | Corporate user | Internet | Backup path used |
| TC-023 | Primary firewall interruption | Corporate user | Approved destination | Secondary firewall active |
| TC-024 | Configuration change | Administrator | FortiGate | Event logged |
| TC-025 | Inter-zone unauthorized traffic | Any internal zone | Another zone | Blocked and logged |

---

# 20. Troubleshooting Commands

The following FortiGate commands may be used in the lab.

## System Status

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

## Firewall Policy Lookup

```shell
diagnose firewall iprope lookup
```

## Session Inspection

```shell
diagnose sys session filter clear
diagnose sys session filter src <SOURCE-IP>
diagnose sys session filter dst <DESTINATION-IP>
diagnose sys session list
```

Clear session filters after troubleshooting:

```shell
diagnose sys session filter clear
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

Stop debug after collecting evidence:

```shell
diagnose debug disable
diagnose debug reset
```

## Packet Capture

```shell
diagnose sniffer packet any 'host <IP-ADDRESS>' 4 0 l
```

Example:

```shell
diagnose sniffer packet any 'host 10.10.50.10 and port 443' 4 0 l
```

## Connectivity Testing

```shell
execute ping <IP-ADDRESS>
execute traceroute <IP-ADDRESS>
```

## High Availability

```shell
get system ha status
diagnose sys ha status
```

> Debugging commands should be used carefully. Debug processes must be stopped
> after troubleshooting to avoid unnecessary resource consumption.

---

# 21. Security Review Checklist

- [ ] Default-deny model is maintained
- [ ] No unnecessary `any` interfaces are used
- [ ] No unnecessary `all` address objects are used
- [ ] No unnecessary `ALL` services are used
- [ ] Management access is isolated
- [ ] Guest access is isolated
- [ ] DMZ access to internal systems is restricted
- [ ] Direct user access to the database is blocked
- [ ] Public services expose only required ports
- [ ] Source NAT is used only where required
- [ ] Original source addresses are preserved internally
- [ ] Security profiles are applied appropriately
- [ ] SSL inspection requirements are documented
- [ ] Allowed traffic is logged
- [ ] High-risk denied traffic is logged
- [ ] Logs are centrally collected
- [ ] Temporary policies have expiration dates
- [ ] Policy ownership is documented
- [ ] Policy comments are complete
- [ ] Policy review dates are defined
- [ ] HA and WAN failover have been tested
- [ ] Configuration backup is available
- [ ] Production information is not included in this repository

---

# 22. Known Limitations

This lab does not currently include:

- FortiManager centralized policy management
- FortiAnalyzer analytics and reporting
- Microsoft Active Directory FSSO
- Microsoft Entra ID integration
- FortiClient EMS integration
- Zero Trust Network Access
- Remote-access VPN
- Site-to-site IPsec VPN
- Full Secure SD-WAN SLA design
- Web Application Firewall
- Network Access Control
- Privileged Access Management
- Automated infrastructure deployment
- Production-scale performance testing

These capabilities may be added in future versions.

---

# 23. Future Improvements

Planned improvements include:

- FortiManager policy-package design
- FortiAnalyzer logging architecture
- Secure SD-WAN performance SLA rules
- Site-to-site IPsec VPN
- SAML authentication with Microsoft Entra ID
- FortiClient EMS and ZTNA integration
- Dedicated DMZ DNS architecture
- WAF protection for the public application
- Threat-intelligence feed integration
- Dynamic external block lists
- Automation stitches
- Infrastructure-as-Code examples
- MITRE ATT&CK-based validation
- Policy-expiration automation
- Compliance mapping
- Security dashboard examples

---

# 24. Repository References

Related project files:

```text
README.md
diagrams/fortigate-enterprise-network-architecture.png
configs/interfaces.conf
configs/address-objects.conf
configs/security-profiles.conf
configs/firewall-policies.conf
configs/vip.conf
configs/ha-sanitized.conf
documentation/firewall-policy-matrix.md
documentation/hardening-checklist.md
documentation/test-plan.md
```

---

# 25. Final Disclaimer

This firewall policy matrix documents a fictional laboratory environment.

The configuration examples are not intended to be copied directly into a
production FortiGate firewall without:

- Technical review
- Security review
- Compatibility validation
- Change approval
- Configuration backup
- Controlled testing
- Rollback planning

No confidential, production, customer, or employer information is included.
