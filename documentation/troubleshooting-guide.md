# FortiGate Lab Troubleshooting Guide

> **Lab Notice**
>
> This document applies to a fictional, sanitized FortiGate Enterprise Security Lab. Execute disruptive tests only during an approved maintenance window with console access, a current backup, and an agreed rollback plan.

## 1. Troubleshooting Method

Use the following sequence:

1. Confirm scope and affected source/destination.
2. Confirm physical and interface status.
3. Confirm VLAN, addressing, ARP, DNS, and routing.
4. Confirm firewall policy match.
5. Confirm NAT or VIP behavior.
6. Confirm session state.
7. Confirm UTM/security-profile behavior.
8. Capture packets.
9. Use debug flow only for a tightly filtered test.
10. Stop all debug activity and document findings.

## 2. Basic Health Checks

```shell
get system status
get system performance status
get system interface physical
get system ha status
diagnose sys ha status
```

## 3. Interface and VLAN Issues

```shell
show system interface
get system interface physical
diagnose netlink interface list
```

Check:

- Correct parent interface `port3`
- Correct VLAN ID
- Correct IP and mask
- Core-switch trunk allows required VLANs
- No unintended Layer 3 gateway on the core switch
- Administrative access enabled only where intended

## 4. Routing and SD-WAN Issues

```shell
get router info routing-table all
get router info routing-table details
diagnose ip route list
diagnose sys sdwan health-check
diagnose sys sdwan service
```

Check:

- Default route references `virtual-wan-link`
- WAN gateways are reachable
- Correct member priority
- SLA targets respond through each link
- No unexpected more-specific route
- Return path is symmetric

## 5. Firewall Policy Match

```shell
show firewall policy
diagnose firewall iprope lookup
```

Validate:

- Source and destination interfaces
- Source and destination objects
- Service object
- Policy order
- Schedule
- NAT setting
- UTM profile references
- Traffic log policy ID

## 6. Session Inspection

```shell
diagnose sys session filter clear
diagnose sys session filter src 10.10.10.10
diagnose sys session filter dst 10.10.20.20
diagnose sys session list
diagnose sys session filter clear
```

Review policy ID, route, NAT, state, duration, and offload information.

## 7. Debug Flow

```shell
diagnose debug reset
diagnose debug flow filter clear
diagnose debug flow filter saddr 10.10.10.10
diagnose debug flow filter daddr 10.10.20.20
diagnose debug flow show function-name enable
diagnose debug flow trace start 100
diagnose debug enable
```

Stop immediately after reproducing the issue:

```shell
diagnose debug disable
diagnose debug reset
```

Common findings:

- `Denied by forward policy check`: no matching allow policy or wrong order
- Route lookup failure: missing or incorrect route
- Reverse path check failure: asymmetric or invalid source routing
- Existing session: stale session may preserve prior policy behavior

## 8. Packet Capture

```shell
diagnose sniffer packet any 'host 10.10.20.20' 4 0 l
diagnose sniffer packet any 'host 10.10.50.10 and port 443' 4 0 l
diagnose sniffer packet any 'host 10.10.60.10 and port 6514' 4 0 l
```

Check whether packets arrive, leave, return, and use expected addresses/ports.

## 9. VIP and Public Portal

```shell
show firewall vip
show firewall policy
get router info routing-table all
```

Check:

- ISP routes `203.0.113.10/32` toward `203.0.113.2`
- VIP external interface is `wan1`
- External and mapped ports are TCP/443
- Mapped host is `10.10.50.10`
- Server default gateway points toward FortiGate
- HTTPS service listens on the server
- No local-in conflict on the public address

## 10. DNS and NTP

```shell
execute ping 10.10.20.10
execute ping 10.10.20.40
```

Check client DNS configuration, UDP/TCP 53 policy, NTP UDP/123 policy, server service state, and logs.

## 11. Security Profile Issues

Check that:

- `utm-status` is enabled
- Profile names exist
- Policy uses flow-based inspection consistently
- FortiGuard connectivity and license are valid
- Certificate inspection is assigned
- The block is visible in the correct security log
- Category IDs and application categories are valid for the build

Do not disable all UTM profiles as the first troubleshooting action. Isolate the specific profile and test safely.

## 12. Syslog Issues

```shell
show log syslogd setting
execute ping-options source 10.10.60.1
execute ping 10.10.60.10
diagnose sniffer packet any 'host 10.10.60.10 and port 6514' 4 0 l
```

Confirm server support for reliable/TLS syslog, destination port, route, source IP, certificate requirements, and receiver logs.

## 13. HA Issues

```shell
get system ha status
diagnose sys ha status
```

Check same FortiOS build, matching group settings, heartbeat links, single active primary, synchronization state, monitored interfaces, and configuration checksum.

## 14. Evidence Template

```text
Incident/Test ID:
Date and time:
Source:
Destination:
Protocol/port:
Observed behavior:
Expected behavior:
Matched policy ID:
Routing result:
Session result:
Packet-capture result:
Debug-flow result:
Root cause:
Corrective action:
Validation result:
Evidence path:
```
