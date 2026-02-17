# Multi-AS Enterprise Network with VLAN Segmentation

A production-grade enterprise network topology implemented in Cisco Packet Tracer, featuring comprehensive routing protocols (OSPF & BGP), VLAN segmentation, and inter-AS connectivity across 6 autonomous systems.

## 🌐 Network Architecture

### Complete Implementation Features
- **6 LANs** with VLAN segmentation (VLAN 10 & VLAN 20 per LAN)
- **OSPF (Area 0)** for intra-AS routing with fast convergence
- **BGP** for inter-AS routing across 6 autonomous systems
- **Router-on-a-stick** configuration for inter-VLAN routing
- **802.1Q trunking** between routers and switches
- **DHCP relay agents** for cross-VLAN IP assignment
- **HTTP web server** in AS4 accessible from all autonomous systems
- **End-to-end connectivity** from AS1 clients to AS4 server

### Network Topology
```
AS1 (Base Network) ←→ AS2 ←→ AS3 ←→ AS4 (Web Server)
                                      ↓
                              AS5 ←→ AS6
```

**AS1 Structure:**
- Central router connecting 6 LANs
- Each LAN split into VLAN 10 (/25) and VLAN 20 (/25)
- Dedicated DHCP servers with relay configuration
- HTTP server for internal services

## 🎯 Key Technical Implementations

### 1. VLAN Segmentation
Each LAN contains two VLANs:
- **VLAN 10** (192.168.x.0/25): PC1 and DHCP server
- **VLAN 20** (192.168.x.128/25): PC2

**Benefits:**
- Isolated broadcast domains for enhanced security
- Logical network separation without additional hardware
- Improved network performance and traffic management

### 2. Inter-VLAN Routing (Router-on-a-Stick)
```
interface FastEthernet0/1.10
 encapsulation dot1Q 10
 ip address 192.168.1.1 255.255.255.128
 ip helper-address 192.168.1.10

interface FastEthernet0/1.20
 encapsulation dot1Q 20
 ip address 192.168.1.129 255.255.255.128
 ip helper-address 192.168.1.10
```

### 3. OSPF Configuration
```
router ospf 1
 network 192.168.1.0 0.0.0.127 area 0
 network 192.168.1.128 0.0.0.127 area 0
 network 172.16.x.0 0.0.0.7 area 0
```

### 4. BGP with Route Redistribution
```
router bgp 1
 neighbor 172.20.1.2 remote-as 2
 network 192.168.0.0 mask 255.255.0.0
 redistribute ospf 1

! Static summary route for VLAN advertisement
ip route 192.168.0.0 255.255.0.0 Null0
```

## 🚀 Getting Started

### Prerequisites
- Cisco Packet Tracer 8.0 or higher
- Basic networking knowledge (subnetting, routing protocols)

### Usage
1. Download `exercise_3.pkt`
2. Open in Cisco Packet Tracer
3. Explore the configurations:
   - View OSPF neighbors: `show ip ospf neighbor`
   - Check BGP sessions: `show ip bgp summary`
   - Verify VLAN config: `show vlan brief`
   - Test connectivity: `ping` from AS1 PC to AS4 server (IP varies)

## 🔍 Technical Highlights

### OSPF (Open Shortest Path First)
- **Link-state routing protocol** with SPF algorithm
- **Area 0 backbone** for all routers within AS1
- **Fast convergence** through LSA flooding
- Advertises /25 VLAN subnets throughout AS1

### BGP (Border Gateway Protocol)
- **Path vector protocol** for inter-AS routing
- **eBGP sessions** between border routers
- **Route advertisement** of 192.168.0.0/16 summary
- **Policy-based routing** for AS path selection

### DHCP Relay
- **ip helper-address** configured on router subinterfaces
- Forwards DHCP broadcasts across VLAN boundaries
- Single DHCP server services multiple VLANs
- Reduces administrative overhead

## 🐛 Key Troubleshooting Solution

**Problem Encountered:** AS1 PCs could not reach AS4 web server despite working BGP sessions.

**Root Cause:** BGP could not automatically advertise /25 VLAN subnets to other autonomous systems.

**Solution Implemented:**
1. Created static summary route: `ip route 192.168.0.0 255.255.0.0 Null0`
2. Advertised summary via BGP: `network 192.168.0.0 mask 255.255.0.0`
3. Redistributed BGP routes into OSPF for internal reachability
4. Result: Full connectivity established between AS1 clients and AS4 server

## 📊 Network Statistics

- **Total Autonomous Systems:** 6
- **LANs in AS1:** 6
- **VLANs per LAN:** 2 (VLAN 10, VLAN 20)
- **Total Subnets:** 14+ across all autonomous systems
- **Routing Protocols:** OSPF (intra-AS), BGP (inter-AS)
- **DHCP Servers:** 6 (one per LAN)

## 🎓 Learning Outcomes

This project demonstrates:
- ✅ Enterprise network design and scalability
- ✅ Dynamic routing protocol configuration (OSPF, BGP)
- ✅ VLAN implementation and trunking (802.1Q)
- ✅ Inter-VLAN routing techniques
- ✅ Route redistribution between protocols
- ✅ Network troubleshooting and problem resolution
- ✅ Multi-AS architecture design

## 📚 Academic Context

**Course:** NYU CS-UY 4793G Computer Networks (Fall 2024)  
**Student:** Oleksandra Kovalenko  
**Instructor:** Badis HAMMI

This implementation represents the culmination of three exercises:
1. Base network with OSPF routing
2. VLAN segmentation with router-on-a-stick
3. Multi-AS BGP routing (this file)

## 🔧 Tools & Technologies

- **Cisco Packet Tracer:** Network simulation and topology design
- **Protocols:** OSPF, BGP, DHCP, HTTP, 802.1Q
- **Devices:** Cisco routers, Layer 2/3 switches, end hosts

## 📄 License

MIT License - Free to use for educational purposes.

## 📧 Contact

Questions? Open an issue on GitHub or connect via LinkedIn.

---

**Note:** This is an educational project. Production networks require additional security measures, redundancy, and monitoring solutions.
