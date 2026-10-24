# FortiGate High-Availability Failover Procedure

> **Lab Notice**
>
> This document applies to a fictional, sanitized FortiGate Enterprise Security Lab. Execute disruptive tests only during an approved maintenance window with console access, a current backup, and an agreed rollback plan.

## 1. Purpose

Validate that the Active-Passive FortiGate cluster transfers firewall services from `FGT-EDGE-01` to `FGT-EDGE-02` without losing configuration integrity, routing, policy enforcement, management access, or centralized logging.

## 2. Scope

- FGCP Active-Passive cluster
- Primary priority: 200
- Secondary priority: 100
- Heartbeat interfaces: `port5` and `port6`
- Monitored interfaces: `wan1`, `wan2`, and `port3`
- Session pickup
- Corporate, server, guest, management, DMZ, and security VLANs

## 3. Preconditions

- [ ] Approved maintenance window
- [ ] Local console access to both members
- [ ] Current encrypted configuration backup
- [ ] Both members run the same supported FortiOS build
- [ ] Cluster reports synchronized status
- [ ] No active critical incident
- [ ] Test clients exist in VLAN 10, VLAN 30, and VLAN 40
- [ ] `log01` can receive events
- [ ] Application owner is available
- [ ] Rollback decision authority is identified

## 4. Baseline Commands

```shell
get system ha status
diagnose sys ha status
get system status
get router info routing-table all
diagnose sys session stat
diagnose sys sdwan health-check
```

Record:

- Active and secondary serial placeholders
- Cluster uptime
- Configuration synchronization state
- Monitored-interface status
- Active session count
- SD-WAN member status

## 5. Baseline Functional Tests

| Test | Expected Result |
|---|---|
| Corporate client accesses approved HTTPS site | Allowed and logged |
| Corporate client accesses `app01:443` | Allowed |
| Guest client accesses internet | Allowed |
| Guest client accesses `app01:443` | Blocked |
| Internet test client accesses portal VIP | Allowed |
| `admin01` accesses management interface | Allowed |
| Security events reach `log01` | Successful |

## 6. Test Scenario HA-01: Controlled Primary Failover

1. Start continuous traffic from approved test clients.
2. Record the current primary member.
3. From console, perform an approved controlled failover or shut down the current primary.
4. Observe cluster election.
5. Confirm `FGT-EDGE-02` becomes primary.
6. Re-run baseline functional tests.
7. Check routing, SD-WAN, VIP, firewall policies, UTM profiles, and logging.
8. Record session interruption and application recovery time.

### Expected Result

- Secondary becomes primary.
- Cluster remains reachable through the management address.
- Policies and objects remain synchronized.
- New sessions are established successfully.
- Existing sessions behave according to session-pickup capability and protocol behavior.
- No unauthorized traffic becomes allowed.
- HA events are logged.

## 7. Test Scenario HA-02: Monitored Interface Failure

1. Confirm `FGT-EDGE-01` is primary.
2. Simulate failure of monitored interface `port3` on the primary only.
3. Observe whether the cluster elects the healthier member.
4. Validate internal VLAN reachability through the new primary.
5. Restore the interface.
6. Confirm stable cluster state after recovery.

## 8. Test Scenario HA-03: Single Heartbeat Failure

1. Disconnect or disable `port5` heartbeat only.
2. Confirm cluster remains operational through `port6`.
3. Confirm no split-brain condition occurs.
4. Verify the heartbeat failure is logged.
5. Restore `port5` and confirm both heartbeat links are healthy.

## 9. Test Scenario HA-04: Configuration Synchronization

1. Make a harmless approved change on the primary, such as updating a lab-only policy comment.
2. Confirm the secondary receives the change.
3. Compare synchronization/checksum state.
4. Revert the harmless change.
5. Confirm both members return to synchronized state.

## 10. Failure Criteria

Stop testing and initiate rollback if:

- Both members become active unexpectedly
- Cluster remains out of sync
- Management access is lost on both members
- Internal routing fails after the expected convergence period
- Public VIP maps to an incorrect server
- Security policies or profiles are missing
- Unauthorized traffic is allowed
- Logging cannot be restored

## 11. Recovery and Rollback

1. Restore heartbeat and monitored interfaces.
2. Reconnect the preferred primary if it was powered down.
3. Confirm both devices detect each other.
4. Wait for synchronization to complete.
5. Avoid forcing failback while the cluster is unstable.
6. If synchronization fails, preserve logs and use the approved backup/recovery procedure.
7. Re-run baseline tests.

## 12. Evidence

Capture sanitized evidence for:

- Pre-test HA status
- Member election
- Post-failover HA status
- Configuration synchronization
- Traffic continuity
- HA event logs
- Restored steady state

## 13. Result Record

| Field | Value |
|---|---|
| Test date | Not tested |
| Tester | Not assigned |
| Initial primary | Not recorded |
| New primary | Not recorded |
| Recovery time | Not measured |
| Session impact | Not measured |
| Overall status | NOT TESTED |
| Evidence path | Pending |
