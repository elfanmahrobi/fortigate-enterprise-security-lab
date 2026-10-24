# Validation Test Results

> **Lab Notice**
>
> This document applies to a fictional, sanitized FortiGate Enterprise Security Lab. Execute disruptive tests only during an approved maintenance window with console access, a current backup, and an agreed rollback plan.

## 1. Status Rule

No test may be marked `PASS` without actual execution and sanitized evidence. Until lab execution, all entries remain `NOT TESTED`.

## 2. Summary

| Status | Count |
|---|---:|
| PASS | 0 |
| FAIL | 0 |
| BLOCKED | 0 |
| NOT TESTED | 25 |

## 3. Test Results

| ID | Test | Expected Result | Actual Result | Status | Evidence |
|---|---|---|---|---|---|
| TC-001 | Corporate HTTPS internet access | Allowed, NAT applied, logged | Pending execution | NOT TESTED | Pending |
| TC-002 | Malicious-category access | Blocked and logged | Pending execution | NOT TESTED | Pending |
| TC-003 | Unauthorized outbound port | Blocked | Pending execution | NOT TESTED | Pending |
| TC-004 | Corporate access to `app01:443` | Allowed | Pending execution | NOT TESTED | Pending |
| TC-005 | Corporate SSH to `app01` | Blocked | Pending execution | NOT TESTED | Pending |
| TC-006 | Corporate DNS to `dc01` | Allowed | Pending execution | NOT TESTED | Pending |
| TC-007 | Corporate access to `db01:5432` | Blocked | Pending execution | NOT TESTED | Pending |
| TC-008 | Guest internet access | Allowed and NAT applied | Pending execution | NOT TESTED | Pending |
| TC-009 | Guest access to internal application | Blocked | Pending execution | NOT TESTED | Pending |
| TC-010 | Guest access to FortiGate management | Blocked | Pending execution | NOT TESTED | Pending |
| TC-011 | Internet access to public portal TCP/443 | Allowed through VIP | Pending execution | NOT TESTED | Pending |
| TC-012 | Internet SSH to public IP | Blocked | Pending execution | NOT TESTED | Pending |
| TC-013 | `web01` access to `db01:5432` | Allowed | Pending execution | NOT TESTED | Pending |
| TC-014 | `web01` SSH to `db01` | Blocked | Pending execution | NOT TESTED | Pending |
| TC-015 | Unapproved DMZ-to-internal traffic | Blocked and logged | Pending execution | NOT TESTED | Pending |
| TC-016 | `admin01` access to FortiGate HTTPS | Allowed | Pending execution | NOT TESTED | Pending |
| TC-017 | Corporate user access to FortiGate HTTPS | Blocked | Pending execution | NOT TESTED | Pending |
| TC-018 | Approved syslog to `log01:6514` | Allowed and received | Pending execution | NOT TESTED | Pending |
| TC-019 | Unauthorized syslog source | Blocked | Pending execution | NOT TESTED | Pending |
| TC-020 | `monitor01` SNMP to approved device | Allowed | Pending execution | NOT TESTED | Pending |
| TC-021 | Unauthorized SNMP request | Blocked | Pending execution | NOT TESTED | Pending |
| TC-022 | Primary WAN failure | New sessions use WAN2 | Pending execution | NOT TESTED | Pending |
| TC-023 | Primary FortiGate failure | Secondary becomes primary | Pending execution | NOT TESTED | Pending |
| TC-024 | Administrator configuration change | Event received by log server | Pending execution | NOT TESTED | Pending |
| TC-025 | Unauthorized inter-zone traffic | Blocked and logged | Pending execution | NOT TESTED | Pending |

## 4. Detailed Result Template

Copy this section for each executed test:

```markdown
### TC-XXX: Test Name

- **Date:**
- **Tester:**
- **Source:**
- **Destination:**
- **Protocol/Port:**
- **Preconditions:**
- **Procedure:**
- **Expected result:**
- **Actual result:**
- **Matched policy:**
- **Log result:**
- **Status:** PASS / FAIL / BLOCKED
- **Evidence:** `../screenshots/<file>`
- **Notes:**
```

## 5. Completion Criteria

Testing is complete only when all required cases have an actual result, evidence link, responsible tester, execution date, and documented remediation for failures.
