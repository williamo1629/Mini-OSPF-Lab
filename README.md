 This is a small GNS3 lab built to demonstrate OSPFv2 single-area convergence, loopback-based Router IDs, cost manipulation, and live Wireshark packet analysis across a 4-router square topology.

##  Table of Contents

- [Overview](#overview)
- [Topology](#topology)
- [IP Addressing](#ip-addressing)
- [Router Configurations](#router-configurations)
- [Verification](#verification)
- [Wireshark Captures](#wireshark-captures)
- [Challenge Lab](#challenge-lab)
- [Tools Used](#tools-used)

##  Overview

This lab was built to demonstrate hands-on routing protocol knowledge. It covers:

| Skill | Details |
|-------|---------|
| OSPFv2 Configuration | Process 1, Area 0, all 4 routers |
| Router ID Assignment | Loopback0 interfaces as stable RIDs |
| Neighbor Adjacency | Full state verified on all links |
| Cost Manipulation | Path influence via ip ospf cost |
| Packet Analysis | Wireshark Hello + LSA captures |
| Troubleshooting | Wildcard mask errors, admin-down interfaces, wrong subnet assignments |

##  Topology
         Lo0: 1.1.1.1                    Lo0: 2.2.2.2
              |                               |
   [R1]──────f0/0──10.0.12.0/30──f0/0──[R2]
    |                                         |
   f0/1                                      f0/1
10.0.13.0/30                           10.0.24.0/30
   f0/0                                      f0/0
    |                                         |
   [R3]──────f0/1──10.0.34.0/30──f0/1──[R4]
              |                               |
         Lo0: 3.3.3.3                    Lo0: 4.4.4.4



All 4 routers in OSPF Area 0 (Backbone) | Process ID: 1


##  IP Addressing

| Link  | Device | Interface | IP Address    |
|-------|--------|-----------|---------------|
| R1–R2 | R1     | f0/0      | 10.0.12.1/30  |
| R1–R2 | R2     | f0/0      | 10.0.12.2/30  |
| R1–R3 | R1     | f0/1      | 10.0.13.1/30  |
| R1–R3 | R3     | f0/0      | 10.0.13.2/30  |
| R2–R4 | R2     | f0/1      | 10.0.24.1/30  |
| R2–R4 | R4     | f0/0      | 10.0.24.2/30  |
| R3–R4 | R3     | f0/1      | 10.0.34.1/30  |
| R3–R4 | R4     | f0/1      | 10.0.34.2/30  |
| Lo0   | R1     | Lo0       | 1.1.1.1/32    |
| Lo0   | R2     | Lo0       | 2.2.2.2/32    |
| Lo0   | R3     | Lo0       | 3.3.3.3/32    |
| Lo0   | R4     | Lo0       | 4.4.4.4/32    |

---

##  Router Configurations

Full configs for each router are in the [`/configs`](./configs) folder.

| File | Router | Role |
|------|--------|------|
| [R1.txt](./configs/R1.txt) | R1 | DR |
| [R2.txt](./configs/R2.txt) | R2 | BDR |
| [R3.txt](./configs/R3.txt) | R3 | DROther |
| [R4.txt](./configs/R4.txt) | R4 | DROther |

---

## ✅ Verification

### OSPF Neighbor Table
show ip ospf neighbor
> 📸 _Screenshot: all neighbors in FULL state goes here_

### OSPF Routing Table
show ip route ospf
> 📸 _Screenshot: full OSPF routing table goes here_

### Key Commands
show ip ospf interface brief   # interface roles + cost
show ip ospf database          # LSA database
ping 4.4.4.4 source 1.1.1.1   # end-to-end reachability
traceroute 4.4.4.4 source 1.1.1.1

##  Wireshark Captures

Capture point: R1–R2 link 

| Filter | Purpose |
|--------|---------|
| ospf | All OSPF traffic |
| ospf.msg.hello 1 | Hello packets (every 10s to 224.0.0.5) |
| ospf.msg.lsupdate| LSA Update floods |










##  Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| GNS3 | 2.2.56 | Network emulation |
| Cisco c3725 | IOS 12.4 | Router image |
| Wireshark | Latest | Packet capture |
| Solar-PuTTY | Free | Console access |

---

## Skills Demonstrated

This lab directly maps to Cisco CCNA 200-301 exam topics:
- OSPFv2 neighbor states and adjacency formation
- OSPF metric calculation and cost manipulation
- Loopback interface best practices for Router IDs
- Wildcard mask configuration
- Network troubleshooting methodology


