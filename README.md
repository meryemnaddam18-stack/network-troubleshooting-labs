# Network Troubleshooting Labs

A collection of network incidents, built in **Cisco Packet Tracer** and documented as
support tickets. Each ticket starts from a user-reported symptom, follows a structured
troubleshooting process, and ends with a verified fix and lessons learned.

## Troubleshooting Method

1. **Scope first:** one user or everyone? Down or just slow? What changed recently?
2. **Work bottom-up through the OSI model**, ruling out one layer at a time:
   - **Physical:** cables, link lights, interface status
   - **Data Link:** VLANs, switch ports, ARP, port security, duplex
   - **Network:** IP addressing, subnet masks, gateway, DHCP, routing
   - **Transport / Application:** ACLs, DNS, services
3. **Compare with something that works:** a working PC, the other end of a link, or the saved baseline.
4. **Prove the root cause with evidence**, fix it, then verify the fix.

The goal is to find the root cause with evidence, not to guess a fix.

## How the Tickets Were Made

- **Practice tickets (TKT-001 to TKT-010):** I introduced each fault myself, then
  troubleshot it from the user's point of view to learn what each failure looks like.
- **Blind tickets (TKT-011 onwards):** I did **not** know the fault in advance. Each one
  was run as an interview-style simulation where I requested commands, questioned the
  "users" and diagnosed the fault only from the output I received. The write-ups record
  my real path, **including wrong turns**. Each fault was then reproduced in Packet Tracer
  for the lab files and screenshots.

Each blind ticket includes a **ticket summary** (work log and closure note to the user),
like a real help desk ticket, followed by the full write-up.

## Base Topology

![Base topology](topology/base-topology.png)

| Device | Role | Address |
|--------|------|---------|
| R1 | Gateway + DHCP server | G0/0: 192.168.1.1/24 · G0/1: 203.0.113.2/30 |
| SW1 | Access switch (2960) | — |
| PC1, PC2 | Clients | DHCP (192.168.1.11+) |
| ISP-R | Simulated ISP / internet | G0/0: 203.0.113.1/30 · G0/1: 198.51.100.1/24 |
| Server | DNS + Web (`www.lab.local`) | 198.51.100.10/24 |

- DHCP pool `192.168.1.0/24`, with `.1–.10` excluded for infrastructure
- Default route on R1 to the ISP; static route on ISP-R back to the office LAN
- Base lab: [`topology/base-lab.pkt`](topology/) · Known-good outputs: [`topology/baseline.txt`](topology/baseline.txt)

## Tickets

### Practice Tickets

| Ticket | Title | Layer | Key Tools | Status |
|--------|-------|-------|-----------|--------|
| [TKT-001](tickets/TKT-001-unplugged-cable/) | Workstation has no network connection | 1 | `ipconfig`, `show interfaces status` | ✅ Resolved |
| [TKT-002](tickets/TKT-002-wrong-vlan/) | One PC cannot reach the network | 2 | `show vlan brief`, `show interfaces status` | ✅ Resolved |
| [TKT-003](tickets/TKT-003-gateway-interface-down/) | All users have no internet, router unreachable *(based on a real incident)* | 1–2 | `arp -a`, `show ip interface brief` | ✅ Resolved |
| [TKT-004](tickets/TKT-004-dhcp-failure/) | Clients cannot get an IP address | 7 (DHCP) | `ipconfig`, `show ip dhcp binding`, static IP test | ✅ Resolved |
| [TKT-005](tickets/TKT-005-wrong-gateway/) | User can reach local devices but not the internet | 3 | `ipconfig /all`, `tracert` | ✅ Resolved |
| [TKT-006](tickets/TKT-006-subnet-mask/) | Some devices reachable, others not | 3 | `ipconfig`, subnet calculation | ✅ Resolved |
| [TKT-007](tickets/TKT-007-duplicate-ip/) | Intermittent connectivity on one workstation | 3 | `show ip arp`, MAC comparison | ✅ Resolved |
| [TKT-008](tickets/TKT-008-missing-route/) | Gateway reachable, internet unreachable | 3 | `show ip route`, `tracert` | ✅ Resolved |
| [TKT-009](tickets/TKT-009-dns-failure/) | Websites don't load, but ping by IP works | 7 | `nslookup`, `ipconfig /all` | ✅ Resolved |
| [TKT-010](tickets/TKT-010-acl-block/) | Ping fails but web browsing works | 3–4 | `show ip interface`, `show access-lists` | ✅ Resolved |

### Blind Tickets 🔒

| Ticket | Title | Layer | Key Tools | Status |
|--------|-------|-------|-----------|--------|
| [TKT-011](tickets/TKT-011-missing-return-route/) | Nobody in the office can open websites | 3 | `ping` from different sources, `show ip route` | ✅ Resolved |
| [TKT-012](tickets/TKT-012-port-security-violation/) | User has no network after returning to desk | 2 | `show interface`, `show port-security` | ✅ Resolved |
| [TKT-013](tickets/TKT-013-duplex-mismatch/) | Internet very slow, large downloads fail | 1–2 | `show interfaces` (duplex, errors) | ✅ Resolved |
| [TKT-014](tickets/TKT-014-web-service-stopped/) | Website down, but the server still pings | 7 | `ping`, local browser test, server services | ✅ Resolved |
| [TKT-015](tickets/TKT-015-dhcp-wrong-gateway/) | One user has no internet, coworker is fine | 3 / 7 | `ipconfig /all`, `show running-config \| section dhcp` | ✅ Resolved |

Status: ⬜ Planned · 🟨 In progress · ✅ Resolved & documented

## Repository Structure

```
topology/        Base lab file, diagram and baseline outputs
tickets/         One folder per ticket (write-up, broken/fixed lab files, screenshots)
templates/       Ticket template used for every write-up
```

## Skills Demonstrated

- Structured, layer-by-layer troubleshooting and problem scoping
- Cisco IOS `show` commands on routers and switches
- Diagnosis of cabling, VLAN, port security, duplex, ARP, IP addressing, DHCP,
  routing, ACL, DNS and application-service faults
- Packet Tracer Simulation mode to follow packets (e.g. DHCP, DNS)
- Clear incident documentation: evidence, root cause, resolution, verification
- User communication through ticket closure notes

## Packet Tracer Limitations

Packet Tracer doesn't always behave like real equipment. Where it differed, the ticket
says so: for example, auto-MDIX in TKT-001, the duplicate-IP check in TKT-007, error
counters in TKT-013 and the err-disabled state not being saved in TKT-012.

## Related Projects

- [Packet Analysis Write-ups](https://github.com/meryemnaddam18-stack/packet-analysis-writeups)
- [Homelab Network](https://github.com/meryemnaddam18-stack/homelab-network)

---
*Author: Meryem Naddam — Computer Programming (Co-op), Algonquin College*
