# SD-WAN Failover Procedure

> **Lab Notice**
>
> This document applies to a fictional, sanitized FortiGate Enterprise Security Lab. Execute disruptive tests only during an approved maintenance window with console access, a current backup, and an agreed rollback plan.

## 1. Purpose

Validate automatic outbound failover from primary ISP `wan1` to secondary ISP `wan2`, as well as stable recovery after `wan1` returns.

## 2. Design Baseline

```text
WAN1 FortiGate IP : 203.0.113.2/30
WAN1 Gateway      : 203.0.113.1
WAN2 FortiGate IP : 198.51.100.2/30
WAN2 Gateway      : 198.51.100.1
SD-WAN rule       : DEFAULT-INTERNET
Performance SLA   : INTERNET-SLA
Preferred member  : WAN1
Backup member     : WAN2
Public VIP        : 203.0.113.10 via WAN1
```

> Outbound failover does not automatically provide inbound failover for the WAN1 public VIP. Secondary inbound publishing requires separate ISP routing, DNS, or load-balancing design.

## 3. Preconditions

- [ ] Both WAN members are up
- [ ] Gateways and addressing match the diagram
- [ ] Default route references the SD-WAN zone
- [ ] Firewall policies reference `virtual-wan-link`
- [ ] SLA has at least two reliable targets
- [ ] Baseline latency and loss recorded
- [ ] Configuration backup completed
- [ ] Console or out-of-band access available

## 4. Baseline Commands

```shell
diagnose sys sdwan health-check
diagnose sys sdwan service
get router info routing-table all
diagnose sys session stat
execute ping-options source 203.0.113.2
execute ping 203.0.113.1
execute ping-options source 198.51.100.2
execute ping 198.51.100.1
```

## 5. Test Scenario SDW-01: Physical WAN1 Failure

1. Start continuous approved HTTPS traffic from VLAN 10.
2. Record the egress public address and active SD-WAN member.
3. Disconnect or administratively disable `wan1` in the controlled lab.
4. Observe SLA and member state.
5. Confirm new outbound sessions use `wan2`.
6. Validate DNS, HTTP, and HTTPS access.
7. Confirm logs identify the secondary path.
8. Restore `wan1`.
9. Observe recovery and preferred-path selection.

### Expected Result

- WAN1 becomes unavailable.
- SD-WAN selects WAN2.
- New outbound sessions succeed through WAN2.
- Guest and corporate internet policies remain enforced.
- WAN1 is selected again only after stable recovery according to configured behavior.

## 6. Test Scenario SDW-02: Upstream Failure with Link Still Up

A cable pull tests interface state but not necessarily SLA path detection. To test the SLA, simulate an upstream reachability failure while keeping the physical WAN interface operational.

1. Preserve current configuration and console access.
2. Introduce an approved lab-only upstream failure, such as blocking SLA targets on the WAN1 path.
3. Confirm `wan1` physical state remains up.
4. Confirm `INTERNET-SLA` marks WAN1 unhealthy.
5. Confirm outbound traffic moves to WAN2.
6. Remove the simulated failure.
7. Confirm stable recovery.

## 7. Test Scenario SDW-03: Single SLA Target Failure

1. Make one SLA target unavailable in the lab.
2. Verify the second target prevents false failure where designed.
3. Confirm WAN1 remains usable if the path is otherwise healthy.
4. Restore the target and record results.

## 8. Test Scenario SDW-04: Both WANs Unavailable

1. Perform only during an approved isolated lab test.
2. Make both WAN paths unavailable.
3. Confirm traffic fails closed and no unintended route appears.
4. Confirm alerts and logs are produced.
5. Restore WAN2 first, then WAN1.
6. Validate controlled recovery.

## 9. Validation Matrix

| Check | Expected Result |
|---|---|
| Corporate internet access through WAN1 | Pass before failure |
| Guest internet access through WAN1 | Pass before failure |
| WAN1 SLA failure detected | Pass |
| New sessions move to WAN2 | Pass |
| Security profiles remain applied | Pass |
| DNS works through WAN2 | Pass |
| Logs show path transition | Pass |
| WAN1 recovery detected | Pass |
| No routing loop or asymmetric path | Pass |

## 10. Failure Criteria

- No default route after failover
- Traffic stays on failed WAN1
- Unauthorized services become allowed
- Security profiles are bypassed
- Routing flaps repeatedly
- Both links are marked down incorrectly
- Management access is lost without console recovery

## 11. Rollback

1. Restore physical and upstream connectivity.
2. Restore original SLA targets and thresholds.
3. Confirm both members are healthy.
4. Restore the approved SD-WAN configuration if modified.
5. Clear test sessions only when approved.
6. Verify routing and policy operation.
7. Record failure evidence before closing the test.

## 12. Result Record

| Field | Value |
|---|---|
| Test date | Not tested |
| WAN1 failure detection time | Not measured |
| WAN2 service recovery time | Not measured |
| WAN1 restoration time | Not measured |
| Packet/session impact | Not measured |
| Overall status | NOT TESTED |
| Evidence path | Pending |
