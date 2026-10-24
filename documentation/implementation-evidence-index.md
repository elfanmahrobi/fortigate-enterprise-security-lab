# Implementation Evidence Index

> **Lab Notice**
>
> This document applies to a fictional, sanitized FortiGate Enterprise Security Lab. Execute disruptive tests only during an approved maintenance window with console access, a current backup, and an agreed rollback plan.

## 1. Purpose

Maintain a traceable index between project controls, executed tests, and sanitized evidence stored in `../screenshots/`.

## 2. Evidence Rules

- Evidence must come from a controlled lab.
- Remove serial numbers, account names, tokens, certificates, UUIDs, MAC addresses, and real identifiers.
- Do not upload production screenshots or logs.
- Preserve enough context to show the tested control.
- Use consistent filenames.
- Do not mark an item complete until reviewed.

## 3. Filename Standard

```text
<sequence>-<control>-<test-id>-<status>.png
```

Examples:

```text
01-ha-status-tc-023-pass.png
02-sdwan-health-tc-022-pass.png
03-guest-deny-tc-009-pass.png
```

## 4. Planned Evidence

| Evidence ID | Control | Related Test | Planned File | Status |
|---|---|---|---|---|
| EV-001 | System dashboard | Baseline | `01-system-dashboard.png` | Pending |
| EV-002 | VLAN interfaces | Baseline | `02-interface-vlan-list.png` | Pending |
| EV-003 | SD-WAN members | TC-022 | `03-sdwan-members.png` | Pending |
| EV-004 | SLA health | TC-022 | `04-sdwan-health-check.png` | Pending |
| EV-005 | Address objects | Baseline | `05-address-objects.png` | Pending |
| EV-006 | Firewall policies | Multiple | `06-firewall-policies.png` | Pending |
| EV-007 | Security profiles | TC-002 | `07-security-profiles.png` | Pending |
| EV-008 | VIP configuration | TC-011 | `08-vip-configuration.png` | Pending |
| EV-009 | Routing table | TC-022 | `09-routing-table.png` | Pending |
| EV-010 | HA status | TC-023 | `10-ha-status.png` | Pending |
| EV-011 | Traffic logs | Multiple | `11-traffic-logs.png` | Pending |
| EV-012 | Security event | TC-002 | `12-security-events.png` | Pending |
| EV-013 | Guest deny log | TC-009 | `13-guest-deny.png` | Pending |
| EV-014 | DB access deny | TC-007 | `14-database-deny.png` | Pending |
| EV-015 | Management restriction | TC-017 | `15-management-deny.png` | Pending |

## 5. Review Record

| Field | Value |
|---|---|
| Evidence reviewer | Not assigned |
| Review date | Pending |
| Sanitization status | Pending |
| Completeness status | Pending Lab Execution |
| Release eligibility | Not ready |

## 6. Sanitization Checklist

- [ ] No real public address
- [ ] No production hostname or domain
- [ ] No real username
- [ ] No serial number
- [ ] No license or contract identifier
- [ ] No certificate subject or private material
- [ ] No authentication token
- [ ] No secret or password
- [ ] No customer or employer name
- [ ] No personal data
- [ ] Screenshot still demonstrates the intended control
