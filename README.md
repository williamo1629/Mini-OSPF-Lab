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

<img width="1284" height="1021" alt="Screenshot 2026-03-17 181226" src="https://github.com/user-attachments/assets/8d637bdc-7859-4b18-9a19-55a0b1b1489f" />



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


##  Router Configurations

Full configs for each router are in the [`/configs`](./configs) folder.

| File | Router | Role |
|------|--------|------|
| [R1.txt](./configs/R1.txt) | R1 | DR |
| [R2.txt](./configs/R2.txt) | R2 | BDR |
| [R3.txt](./configs/R3.txt) | R3 | DROther |
| [R4.txt](./configs/R4.txt) | R4 | DROther |


##  Verification

### OSPF Neighbor Table
show ip ospf neighbor
<img width="888" height="658" alt="Screenshot 2026-03-17 175316" src="https://github.com/user-attachments/assets/62a985c6-383f-4d34-87ea-abff9975a5db" />

<img width="888" height="663" alt="Screenshot 2026-03-17 180143" src="https://github.com/user-attachments/assets/6583a74b-2069-473a-a0b5-92b338771dc5" />


### OSPF Routing Table
show ip route ospf
<img width="870" height="239" alt="Screenshot 2026-03-17 180437" src="https://github.com/user-attachments/assets/fbf0e21d-82d6-4feb-b768-fc325acd1b7b" />

<img width="889" height="331" alt="Screenshot 2026-03-17 180532" src="https://github.com/user-attachments/assets/3f2ef605-4036-4709-893d-87e02b31a640" />




### Key Commands
show ip ospf interface brief  -  # interface roles + cost


show ip ospf database         -  # LSA database


ping 4.4.4.4 source 1.1.1.1   - # end-to-end reachability


traceroute 4.4.4.4 source 1.1.1.1




##  Wireshark Captures

Capture point: R1–R2 link 

| Filter | Purpose |
|--------|---------|
| ospf | All OSPF traffic |
| ospf.msg.hello 1 | Hello packets (every 10s to 224.0.0.5) |
<img width="1503" height="1023" alt="ospf msg hello" src="https://github.com/user-attachments/assets/4ff36955-3512-41cc-9bb7-8e6e8fbcc75c" />

| ospf.msg.lsupdate| LSA Update floods |
<img width="1894" height="1026" alt="ospf msg lsupdate" src="https://github.com/user-attachments/assets/c8f81b24-95fd-4319-957d-cd72606d9807" />










##  Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| GNS3 | 2.2.56 | Network emulation |
| Cisco c3725 | IOS 12.4 | Router Image|
| Wireshark | Latest | Packet capture |
| Solar-PuTTY | Free | Console access |


*Requires Cisco c3725 IOS image to run locally*

## Skills Demonstrated

This lab directly maps to:
- OSPFv2 neighbor states and adjacency formation
- OSPF metric calculation and cost manipulation
- Loopback interface best practices for Router IDs
- Wildcard mask configuration
- Network troubleshooting methodology


