
# Network Troubleshooting Guide

## Overview

This repository provides practical documentation for diagnosing common network connectivity and infrastructure issues.

It covers structured troubleshooting approaches for Cisco switches, VLANs, IP addressing, DNS, DHCP, routing, network performance, and endpoint connectivity.

The examples are intended for learning, lab environments, and general technical reference.

No production network configurations or confidential infrastructure details are included.

---

## Technologies and Tools

- Cisco Catalyst Switches
- TCP/IP Networking
- IPv4 Addressing and Subnetting
- VLANs and Trunking
- DNS
- DHCP
- ICMP
- ARP
- Ethernet
- Windows PowerShell
- Windows Command Prompt
- Cisco IOS CLI
- Network Monitoring Tools
- Wireshark

---

## Network Troubleshooting Methodology

A structured troubleshooting approach helps identify the root cause of network issues.

### Standard Workflow

1. Identify the reported problem.
2. Determine the affected users, devices, or services.
3. Confirm the scope and impact.
4. Check physical connectivity.
5. Review IP addressing and network configuration.
6. Test local network communication.
7. Verify gateway connectivity.
8. Test communication to the destination.
9. Review DNS resolution where applicable.
10. Investigate switching, VLAN, or routing issues.
11. Apply an approved corrective action.
12. Verify connectivity and document the result.

### Troubleshooting Principles

- Start with simple checks before investigating complex causes.
- Separate physical, network, and application issues.
- Compare expected behavior with observed behavior.
- Change one variable at a time where practical.
- Record test results.
- Follow approved change management procedures.

---

## OSI Model Troubleshooting

The OSI model provides a useful framework for isolating network issues.

| Layer | Name | Common Troubleshooting Area |
|---|---|---|
| 1 | Physical | Cabling, interfaces, link status |
| 2 | Data Link | VLANs, MAC addresses, switching |
| 3 | Network | IP addresses, gateways, routing |
| 4 | Transport | TCP/UDP ports, connectivity |
| 5 | Session | Session establishment |
| 6 | Presentation | Data formatting and encryption |
| 7 | Application | DNS, HTTP, application services |

In practice, troubleshooting often moves between layers based on the symptoms.

---

## Basic Windows Network Commands

### Display IP Configuration

```powershell
ipconfig /all
```

Review:

- IPv4 address
- Subnet mask
- Default gateway
- DNS servers
- DHCP status
- Network adapter details

### Test Network Connectivity

```powershell
ping 192.0.2.1
```

`192.0.2.1` is a documentation-only example address. Replace it with an authorized test target when working in a lab.

A failed ping does not always mean a device is unavailable because ICMP may be blocked.

### Trace Network Path

```powershell
tracert example.com
```

This command displays responding network hops toward the destination.

Some routers may not respond to traceroute probes.

### Check DNS Resolution

```powershell
nslookup example.com
```

### Display Local Routing Table

```powershell
route print
```

### Display ARP Cache

```powershell
arp -a
```

### Display Active Connections

```powershell
netstat -ano
```

---

## PowerShell Network Troubleshooting

### Test Connectivity

```powershell
Test-Connection -ComputerName "example.com" -Count 4
```

### Test TCP Port Connectivity

```powershell
Test-NetConnection -ComputerName "example.com" -Port 443
```

This checks whether a TCP connection to the specified port can be established.

### Check DNS Resolution

```powershell
Resolve-DnsName "example.com"
```

### View Network Adapter Status

```powershell
Get-NetAdapter |
    Select-Object Name, Status, LinkSpeed
```

### Review IP Configuration

```powershell
Get-NetIPConfiguration
```

### View IPv4 Routes

```powershell
Get-NetRoute -AddressFamily IPv4
```

These commands are intended for general diagnostics and do not modify network settings.

---

## Cisco Switch Troubleshooting

Cisco Catalyst switches provide commands for reviewing interface status, VLAN configuration, and switching information.

The following examples are read-only commands.

### Check Interface Status

```text
show interfaces status
```

Review:

- Interface status
- Connected ports
- VLAN assignment
- Interface speed
- Duplex information

### Check Interface Details

```text
show interfaces
```

Look for:

- Interface errors
- Input errors
- Output errors
- CRC errors
- Interface resets
- Traffic statistics

### Check VLANs

```text
show vlan brief
```

This displays VLAN information on supported Cisco IOS platforms.

### Check Trunk Interfaces

```text
show interfaces trunk
```

Review trunk status and permitted VLANs.

### Check MAC Address Table

```text
show mac address-table
```

This displays learned MAC addresses and associated interfaces.

### Check Switch Information

```text
show version
```

### Check Neighbor Information

```text
show cdp neighbors
```

CDP availability depends on device configuration.

Use these commands only on devices you are authorized to administer.

---

## VLAN Troubleshooting

VLANs separate Layer 2 broadcast domains.

Incorrect VLAN configuration can cause connectivity issues.

### Common VLAN Problems

- Incorrect access VLAN
- VLAN missing from a switch
- VLAN not permitted on a trunk
- Trunk negotiation or configuration issues
- Incorrect endpoint IP configuration
- Missing or incorrect inter-VLAN routing
- Interface shutdown or link problems

### VLAN Investigation Workflow

1. Identify the affected endpoint.
2. Check the physical connection.
3. Review the access interface status.
4. Confirm the expected VLAN.
5. Review VLAN availability.
6. Check trunk status where relevant.
7. Verify gateway and routing configuration.
8. Test connectivity after approved remediation.

### Useful Cisco Commands

```text
show vlan brief
show interfaces status
show interfaces trunk
show mac address-table
```

Do not publish production VLAN IDs, VLAN names, interface mappings, or network topology details.

---

## IP Addressing and Subnetting

Incorrect IP settings can prevent communication.

Important values include:

- IP address
- Subnet mask
- Default gateway
- DNS server
- Network prefix

### Documentation Example

| Setting | Example |
|---|---|
| Network | 192.0.2.0/24 |
| Host IP | 192.0.2.10 |
| Subnet Mask | 255.255.255.0 |
| Example Gateway | 192.0.2.1 |
| DNS Test Domain | example.com |

These values are fictional examples, not real company settings.

### IP Address Troubleshooting

1. Confirm the assigned IP address.
2. Check the subnet mask.
3. Verify the expected gateway.
4. Identify possible duplicate IP addresses.
5. Review DHCP assignment if applicable.
6. Test local and gateway connectivity.
7. Investigate routing or DNS if local connectivity works.

---

## DHCP Troubleshooting

DHCP automatically provides IP configuration to clients.

### Common DHCP Issues

- DHCP server unavailable
- Address pool exhausted
- DHCP relay configuration problems
- Client connected to an incorrect VLAN
- Network connectivity problems
- Incorrect DHCP scope options
- DHCP service problems

### Windows DHCP Client Checks

```powershell
ipconfig /all
```

Check whether the endpoint received the expected address configuration.

### DHCP Troubleshooting Workflow

1. Confirm the network adapter is connected.
2. Review the assigned IP address.
3. Identify whether the address was obtained through DHCP.
4. Check the expected VLAN.
5. Verify DHCP server availability through approved tools.
6. Review relay configuration where applicable.
7. Investigate scope availability and configuration.
8. Confirm connectivity after remediation.

Renewing a DHCP lease can interrupt connectivity and should be performed only when appropriate.

---

## DNS Troubleshooting

DNS translates domain names into IP addresses and supports many enterprise applications.

### Common DNS Problems

- Incorrect DNS server configuration
- Missing DNS records
- Outdated DNS records
- DNS server unavailable
- Network connectivity problems
- DNS caching issues

### DNS Testing

```powershell
nslookup example.com
```

```powershell
Resolve-DnsName "example.com"
```

### DNS Troubleshooting Workflow

1. Confirm network connectivity.
2. Review the configured DNS servers.
3. Test resolution of the required hostname.
4. Compare results with expected DNS records.
5. Check whether the issue affects one device or multiple devices.
6. Review DNS server health through approved administration tools.
7. Verify resolution after remediation.

---

## Default Gateway and Routing Issues

A default gateway allows a device to send traffic to destinations outside its local network.

### Common Routing Problems

- Incorrect default gateway
- Missing route
- Incorrect subnet configuration
- Routing device unavailable
- Firewall restrictions
- Network path problems

### Check Routing Table on Windows

```powershell
route print
```

### Check Cisco Routing Information

On supported Layer 3 Cisco devices:

```text
show ip route
```

### Routing Troubleshooting Workflow

1. Confirm the source IP address and subnet.
2. Verify the default gateway.
3. Test gateway reachability.
4. Review the local routing table.
5. Investigate intermediate network devices.
6. Review firewall and access restrictions.
7. Confirm return-path routing where relevant.
8. Verify connectivity after remediation.

---

## Network Performance Troubleshooting

Network performance problems may include:

- High latency
- Packet loss
- Interface congestion
- Interface errors
- Duplex mismatches
- Bandwidth saturation
- DNS delays
- Application response delays

### Recommended Checks

1. Identify when the problem occurs.
2. Determine the affected systems.
3. Review interface statistics.
4. Check bandwidth utilization.
5. Test latency and packet loss.
6. Compare current and historical monitoring data.
7. Investigate relevant network devices.
8. Document the findings.

### Cisco Interface Statistics

```text
show interfaces
```

### Windows Connectivity Test

```powershell
Test-Connection -ComputerName "example.com" -Count 10
```

Results should be interpreted with consideration for network conditions and ICMP filtering.

---

## Common Troubleshooting Scenarios

### Scenario 1: Device Cannot Reach Its Gateway

Possible causes:

- Physical link problem
- Incorrect IP configuration
- Incorrect VLAN
- Gateway unavailable
- Interface restrictions

Recommended investigation:

1. Check network adapter status.
2. Verify IP configuration.
3. Confirm the expected VLAN.
4. Review switch interface status.
5. Test gateway connectivity.
6. Investigate the network path.

### Scenario 2: IP Connectivity Works but DNS Fails

Possible causes:

- Incorrect DNS settings
- DNS server unavailable
- Missing or incorrect DNS records
- DNS filtering or connectivity problems

Recommended investigation:

1. Test connectivity using an authorized IP target.
2. Check DNS server configuration.
3. Run `nslookup`.
4. Compare expected and actual DNS responses.
5. Investigate DNS service availability.

### Scenario 3: Endpoint Connected to Wrong VLAN

Possible symptoms:

- Unexpected IP address
- Gateway unreachable
- Inability to access required services
- DHCP assignment from an unexpected subnet

Recommended investigation:

1. Identify the endpoint connection.
2. Review access interface status.
3. Verify the expected VLAN.
4. Review trunk configuration if relevant.
5. Confirm the endpoint IP settings.
6. Apply changes only through approved procedures.

### Scenario 4: Intermittent Network Connectivity

Possible causes:

- Interface errors
- Physical cable problems
- Network congestion
- Unstable links
- Endpoint driver problems
- Network device issues

Recommended investigation:

1. Identify the frequency of the issue.
2. Check interface error counters.
3. Review link status changes.
4. Compare monitoring history.
5. Check endpoint logs.
6. Investigate the affected network segment.

---

## Example Troubleshooting Record

The following is a fictional scenario.

| Field | Example |
|---|---|
| Device | LAB-PC-01 |
| Environment | Test |
| Issue | Gateway connectivity failure |
| Physical Link | Connected |
| IP Configuration | Reviewed |
| VLAN Assignment | Incorrect in lab |
| Corrective Action | Updated approved lab configuration |
| Verification | Connectivity restored |
| Final Status | Resolved |

This example does not represent an actual production incident.

---

## Network Troubleshooting Checklist

- [ ] Identify the affected device or service
- [ ] Confirm the scope of the issue
- [ ] Check physical connectivity
- [ ] Review network adapter status
- [ ] Verify IP address and subnet mask
- [ ] Confirm default gateway
- [ ] Check DNS resolution
- [ ] Review VLAN assignment
- [ ] Check switch interface status
- [ ] Review routing information
- [ ] Investigate firewall restrictions
- [ ] Check monitoring alerts
- [ ] Verify connectivity after remediation
- [ ] Document the final outcome

---

## Best Practices

- Follow a structured troubleshooting process.
- Use read-only diagnostic commands where possible.
- Apply least-privilege access principles.
- Avoid unnecessary configuration changes.
- Use approved change management procedures.
- Maintain accurate network documentation.
- Review monitoring and event information.
- Validate the results of corrective actions.
- Escalate unresolved issues appropriately.
- Protect sensitive infrastructure information.

---

## Security and Confidentiality

This repository contains general technical documentation and illustrative examples.

Do not publish:

- Real production IP addresses
- Internal network subnets
- Switch configuration backups
- Actual VLAN-to-IP mappings
- Production device hostnames
- SNMP authentication credentials
- Firewall or access control configurations
- Confidential network topology diagrams
- Internal DNS records
- Company-specific infrastructure documentation

Documentation-only IP address ranges are used where examples are necessary.

---

## Conclusion

Network troubleshooting requires a systematic approach to identifying connectivity, switching, addressing, routing, and application-related problems.

Using structured diagnostics, appropriate monitoring tools, and clear documentation helps administrators investigate network issues effectively.

This repository provides a practical reference for common enterprise networking troubleshooting activities.
