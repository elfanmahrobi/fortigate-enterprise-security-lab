# Configuration Rollback Plan

> **Lab Notice**
>
> This document applies to a fictional, sanitized FortiGate Enterprise Security Lab. Execute disruptive tests only during an approved maintenance window with console access, a current backup, and an agreed rollback plan.

## 1. Purpose

Provide a controlled method to restore the FortiGate lab to its last known-good state when implementation or validation causes unacceptable impact.

## 2. Rollback Decision Criteria

Initiate rollback when one or more conditions occur:

- Management access is lost and cannot be safely restored
- Corporate or guest internet access fails unexpectedly
- Internal business application access fails
- Public VIP exposes the wrong service or server
- Routing loop or persistent asymmetry occurs
- HA cluster becomes unstable or out of sync
- SD-WAN continuously flaps
- Security profiles block approved critical traffic without a safe exception
- Unauthorized traffic is allowed
- Central logging is materially disrupted
- Recovery exceeds the approved maintenance window

## 3. Roles

| Role | Responsibility |
|---|---|
| Change Owner | Coordinates implementation and rollback decision |
| Security Engineer | Restores policies, objects, profiles, VIP, and management controls |
| Network Engineer | Restores interfaces, VLANs, routing, SD-WAN, and HA |
| Application Owner | Validates application functionality |
| Test Coordinator | Records test and recovery evidence |
| Approver | Accepts service restoration and closure |

## 4. Required Artifacts

- Encrypted pre-change FortiGate backup
- Exported relevant configuration sections
- Current architecture diagram
- Policy matrix
- Implementation plan
- Console-access details stored outside GitHub
- Vendor support details where applicable
- Known-good validation results

## 5. Pre-Change Backup Checklist

- [ ] Record FortiOS version and build
- [ ] Record HA status
- [ ] Export encrypted full configuration
- [ ] Export relevant CLI sections
- [ ] Save routing table
- [ ] Save SD-WAN status
- [ ] Save policy and object state
- [ ] Save UTM profile references
- [ ] Save VIP configuration
- [ ] Confirm backup can be accessed by authorized personnel

## 6. Component Rollback Procedures

### 6.1 Firewall Policy

1. Disable the newly introduced rule.
2. Re-enable the previous approved rule if it was disabled.
3. Restore previous policy order.
4. Clear affected test sessions only if required.
5. Re-test both allowed and denied flows.

### 6.2 Address or Service Object

1. Restore the prior object value.
2. Confirm dependencies before deleting a new object.
3. Validate all referencing policies.
4. Confirm no unrelated policy is affected.

### 6.3 Security Profile

1. Restore the prior profile assignment.
2. If needed, detach only the problematic new profile after approval.
3. Do not disable all inspection globally.
4. Validate business traffic and security logging.

### 6.4 SD-WAN and Routing

1. Restore original member, SLA, and rule configuration.
2. Restore the original default route.
3. Confirm gateway reachability.
4. Confirm routing table and active member.
5. Verify both corporate and guest internet traffic.

### 6.5 Virtual IP

1. Disable the inbound policy.
2. Restore the previous VIP mapping.
3. Confirm public routing and internal server address.
4. Verify only approved TCP/443 exposure.
5. Check that SSH, RDP, and other ports remain blocked.

### 6.6 Management Access

1. Use console access.
2. Restore the prior management interface address and `allowaccess` options.
3. Restore administrator trusted hosts and local-in policy.
4. Verify access from `HOST-ADMIN01` only.
5. Confirm WAN management remains disabled.

### 6.7 High Availability

1. Do not alter both members simultaneously.
2. Restore heartbeat and monitored interfaces.
3. Confirm a single primary exists.
4. Wait for synchronization.
5. Restore the known-good HA configuration if required.
6. Escalate if split-brain or persistent out-of-sync status remains.

### 6.8 Full Configuration Restore

Use only when targeted rollback is insufficient.

1. Preserve failure evidence.
2. Confirm backup matches model, VDOM mode, and FortiOS expectations.
3. Restore through approved GUI or CLI process.
4. Allow device or cluster restart if required.
5. Reconnect through console.
6. Validate interfaces, routing, policies, HA, logging, and applications.

## 7. Post-Rollback Validation

- [ ] Management access restored
- [ ] HA cluster healthy and synchronized
- [ ] SD-WAN members healthy
- [ ] Default route present
- [ ] Corporate internet access works
- [ ] Guest internet access works
- [ ] Guest internal access is blocked
- [ ] Internal application access works
- [ ] Direct user database access is blocked
- [ ] Public portal behavior matches known-good state
- [ ] Central logs are received
- [ ] No unexpected critical events remain

## 8. Communication Record

```text
Change reference:
Rollback decision time:
Decision authority:
Reason:
Services affected:
Recovery start:
Recovery complete:
Validation owner:
Final status:
Follow-up action:
```

## 9. Completion Criteria

Rollback is complete only after technical recovery, application-owner validation, security validation, evidence capture, and change-record update.
