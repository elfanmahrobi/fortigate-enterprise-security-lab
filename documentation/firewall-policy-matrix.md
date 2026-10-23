# Firewall Policy Matrix

This document defines the proposed firewall-policy requirements for the
FortiGate Enterprise Security Lab.

All networks, systems, domain names, and IP addresses in this document are
fictional and used only for educational and portfolio purposes.

---

## Policy Design Principles

The firewall policies in this lab follow these principles:

- Default-deny access control
- Least-privilege access
- Explicit source and destination definition
- Specific service restriction
- Network segmentation
- Management-plane isolation
- DMZ isolation
- Security-profile inspection
- Traffic and security-event logging
- Documented business justification

Policies should be ordered from the most specific rule to the most general
rule.

---

## Network Objects

| Object Name | Network or Host | Description |
|---|---|---|
| `NET-CORP-USERS` | `10.10.10.0/24` | Corporate user network |
| `NET-INTERNAL-SERVERS` | `10.10.20.0/24` | Internal server network |
| `NET-GUEST` | `10.10.30.0/24` | Guest wireless network |
| `NET-MANAGEMENT` | `10.10.40.0/24` | Administrative network |
| `NET-DMZ` | `10.10.50.0/24` | Public-service DMZ |
| `NET-SECURITY` | `10.10.60.0/24` | Logging and monitoring network |
| `HOST-DC01` | `10.10.20.10/32` | Active Directory and DNS server |
| `HOST-APP01` | `10.10.20.20/32` | Internal web application |
| `HOST-DB01` | `10.10.20.30/32` | Internal database server |
| `HOST-WEB01` | `10.10.50.10/32` | Public web server |
| `HOST-ADMIN01` | `10.10.40.10/32` | Administrative workstation |
| `HOST-LOG01` | `10.10.60.10/32` | Central logging server |
| `RFC1918-NETWORKS` | Private address ranges | Internal private networks |

---

## Firewall Policy Summary

| Policy ID | Policy Name | Source Zone | Destination Zone | Source | Destination | Service | Action | NAT | Logging |
|---:|---|---|---|---|---|---|---|---|---|
| 10 | `CORP-USERS-to-INTERNET` | CORP-USERS | WAN | `NET-CORP-USERS` | All | DNS, HTTP, HTTPS | Allow | Enabled | All sessions |
| 20 | `CORP-USERS-to-APP01` | CORP-USERS | INTERNAL-SERVERS | `NET-CORP-USERS` | `HOST-APP01` | HTTPS | Allow | Disabled | All sessions |
| 21 | `CORP-USERS-to-DC01-DNS` | CORP-USERS | INTERNAL-SERVERS | `NET-CORP-USERS` | `HOST-DC01` | DNS | Allow | Disabled | All sessions |
| 22 | `CORP-USERS-to-DB01-DENY` | CORP-USERS | INTERNAL-SERVERS | `NET-CORP-USERS` | `HOST-DB01` | All | Deny | Disabled | All sessions |
| 30 | `GUEST-to-INTERNET` | GUEST | WAN | `NET-GUEST` | All | DNS, HTTP, HTTPS | Allow | Enabled | All sessions |
| 31 | `GUEST-to-PRIVATE-DENY` | GUEST | Internal zones | `NET-GUEST` | `RFC1918-NETWORKS` | All | Deny | Disabled | All sessions |
| 40 | `INTERNET-to-DMZ-PORTAL` | WAN1 | DMZ | All | `VIP-PORTAL-HTTPS` | HTTPS | Allow | VIP/DNAT | All sessions |
| 50 | `WEB01-to-DB01` | DMZ | INTERNAL-SERVERS | `HOST-WEB01` | `HOST-DB01` | PostgreSQL | Allow | Disabled | All sessions |
| 51 | `DMZ-to-INTERNAL-DENY` | DMZ | INTERNAL-SERVERS | `NET-DMZ` | `NET-INTERNAL-SERVERS` | All | Deny | Disabled | All sessions |
| 60 | `DEVICES-to-LOG01` | Internal zones | SECURITY | Approved devices | `HOST-LOG01` | Syslog TLS | Allow | Disabled | All sessions |
| 70 | `ADMIN01-to-FORTIGATE` | MANAGEMENT | FortiGate | `HOST-ADMIN01` | FortiGate | HTTPS, SSH, PING | Allow | Disabled | Administrative logs |
| 71 | `NON-MGMT-to-FORTIGATE-DENY` | Other zones | FortiGate | All internal networks | FortiGate | Administrative services | Deny | Disabled | All sessions |
| 100 | `INTERZONE-DEFAULT-DENY` | Any | Any | All | All | All | Deny | Disabled | All sessions |

---

## Policy 10: Corporate Users to Internet

### Purpose

Allow corporate users to access approved internet services while applying
security inspection.

### Traffic Definition

```text
Source interface      : VLAN10-USERS
Destination interface : SD-WAN or WAN zone
Source address        : NET-CORP-USERS
Destination address   : All
Services              : DNS, HTTP, HTTPS
Action                : Allow
Source NAT            : Enabled
Logging               : All sessions
