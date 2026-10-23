# FortiGate Enterprise Security Lab - Sanitized Configurations

These files implement the dummy architecture documented in the project README and policy matrix.

## Target baseline

- FortiOS 7.6.x lab baseline
- Root VDOM
- `wan1`: `203.0.113.2/30`, gateway `203.0.113.1`
- `wan2`: `198.51.100.2/30`, gateway `198.51.100.1`
- `port3`: 802.1Q trunk to the core switch
- `port5` and `port6`: HA heartbeat interfaces
- VLANs 10, 20, 30, 40, 50, and 60
- Public VIP `203.0.113.10` is a documentation-only routed public address

## Important

This is a sanitized portfolio template, not a production-ready configuration. Interface names, available FortiGuard categories, profile options, HA behavior, SD-WAN members, administrator settings, certificates, and logging features vary by model, license, and FortiOS build.

Do not paste the complete folder blindly. Review each file and apply it in the order below from console access in a disposable lab.

## Recommended order

1. `01-system-settings.conf`
2. `02-interfaces-vlans.conf`
3. `03-address-objects.conf`
4. `04-service-objects.conf`
5. `05-security-profiles.conf`
6. `06-sdwan.conf`
7. `07-routing.conf`
8. `08-vip.conf`
9. `09-firewall-policies.conf`
10. `10-local-in-management.conf`
11. `11-logging.conf`
12. `12-ha-primary.conf` on FGT-EDGE-01
13. `13-ha-secondary.conf` on FGT-EDGE-02
14. `14-validation-commands.txt`

## Pre-flight

- Keep console access available.
- Back up the configuration.
- Confirm interface names with `show system interface`.
- Confirm FortiGuard licensing before applying custom UTM profiles.
- Create a real administrator password and HA password manually.
- Confirm `log01` supports Syslog over TLS before enabling reliable mode.
- Replace example DNS and NTP values where required.
- Validate policy order and routing after every stage.
