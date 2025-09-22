# Task 4: Docker Networking with SONiC Routers 🌐

## Using SONiC to Build Virtual Router Labs

In this task, we'll use **SONiC (Software for Open Networking in the Cloud)** containers to **bring up virtual routers**, **configure routing protocols**, and **validate network connectivity**. This simulates real enterprise networking scenarios in a containerized environment.

**What You'll Accomplish:**
- **Deploy SONiC router containers** as virtual network devices
- **Configure routing interfaces** with IP addresses and subnets
- **Set up routing protocols** (OSPF, static routes) between routers
- **Validate routing tables** and verify route propagation
- **Test connectivity** using ping, traceroute, and network diagnostics
- **Simulate network failures** and validate failover scenarios

**SONiC (Software for Open Networking in the Cloud)** is Microsoft's open-source network operating system that powers:

### **Enterprise Adoption**
- **Microsoft Azure**: Azure networking uses SONiC
- **Major Cloud Providers**: Google, Facebook, Alibaba use SONiC-based solutions  
- **Enterprise Data Centers**: Dell, HPE, Cisco offer SONiC-compatible switches
- **Open Networking**: Disaggregated hardware with standardized software

### **Why This Matters for Network Engineers**
- **Router Configuration Skills**: Learn to configure real network operating systems
- **Route Validation**: Practice verifying routing protocols and connectivity
- **Lab Environment**: Build complex network topologies without physical hardware
- **Enterprise Preparation**: Experience with production-grade network OS
- **Cost Efficiency**: Open-source alternative to proprietary router simulators

---

## Step 1: SONiC Container Setup

### Pull SONiC Images
```bash
# Pull official SONiC container image
docker pull sonicdev/sonic-vs:latest

# Alternative: Use specific SONiC version
docker pull sonicdev/sonic-vs:202211

# Verify image is downloaded
docker images | grep sonic
```

### Create SONiC Network Environment
```bash
# Create custom Docker network for our lab
docker network create --driver bridge --subnet=172.20.0.0/16 sonic-lab

# Verify network creation
docker network ls
docker network inspect sonic-lab
```

---

## Step 2: Launch Two SONiC Devices

### Start First SONiC Device (R1)
```bash
# Launch SONiC Router 1
docker run -d \
  --name sonic-r1 \
  --hostname R1 \
  --network sonic-lab \
  --ip 172.20.1.1 \
  --privileged \
  --cap-add=NET_ADMIN \
  sonicdev/sonic-vs:latest

# Verify R1 is running
docker ps | grep sonic-r1
```

### Start Second SONiC Device (R2)
```bash
# Launch SONiC Router 2
docker run -d \
  --name sonic-r2 \
  --hostname R2 \
  --network sonic-lab \
  --ip 172.20.1.2 \
  --privileged \
  --cap-add=NET_ADMIN \
  sonicdev/sonic-vs:latest

# Verify R2 is running
docker ps | grep sonic-r2
```

---

## Step 3: Create Multiple Links Between Devices

### Create Additional Network Bridges
```bash
# Create multiple point-to-point networks
docker network create --driver bridge --subnet=10.1.1.0/30 link1
docker network create --driver bridge --subnet=10.1.2.0/30 link2
docker network create --driver bridge --subnet=10.1.3.0/30 link3

# Verify networks
docker network ls | grep link
```

### Connect Devices to Multiple Links
```bash
# Connect R1 and R2 to Link 1
docker network connect link1 sonic-r1
docker network connect link1 sonic-r2

# Connect R1 and R2 to Link 2  
docker network connect link2 sonic-r1
docker network connect link2 sonic-r2

# Connect R1 and R2 to Link 3
docker network connect link3 sonic-r1  
docker network connect link3 sonic-r2
```

---

## Step 4: Interface Discovery and Configuration

### Access SONiC Devices
```bash
# Access R1 console
docker exec -it sonic-r1 bash

# Access R2 console (open new terminal)
docker exec -it sonic-r2 bash
```

### Discover Network Interfaces
```bash


# Alternative: Use Linux commands
ip addr show
ifconfig -a
#You can copy paste this into chatgpt/claude.ai to understand the interface and ip addresses

Find 
---

## Step 5: Network Connectivity Testing

### Basic Connectivity Tests
```bash
# From R1 - Test connectivity to R2
ping -c 3 10.1.1.2    # Link 1
ping -c 3 10.1.2.2    # Link 2  
ping -c 3 10.1.3.2    # Link 3

# Check routing table
show ip route
ip route show

# Test from R2 to R1
ping -c 3 10.1.1.1    # Link 1
ping -c 3 10.1.2.1    # Link 2
ping -c 3 10.1.3.1    # Link 3
```

### Interface Shutdown Testing
```bash
# From R1 - Shutdown one interface
sudo config interface shutdown [FILL HERE] veth*

# Test connectivity (should fail on Link 1)
ping -c 3 10.1.1.2    # Should fail
ping -c 3 10.1.2.2    # Should work
ping -c 3 10.1.3.2    # Should work

# Bring interface back up
sudo config interface startup Ethernet0

# Verify connectivity restored
ping -c 3 10.1.1.2    # Should work again
```

---

## Step 6: External Connectivity Testing

### Configure Default Gateway
```bash
# From host machine - Check Docker network gateway
docker network inspect sonic-lab | grep Gateway

# Inside R1 and R2 - Add default route
sudo config route add prefix 0.0.0.0/0 nexthop 172.20.0.1

# Verify route added
show ip route
```

### Test Internet Connectivity
```bash
# Test DNS resolution and connectivity
ping -c 3 8.8.8.8         # Google DNS
ping -c 3 google.com      # Google website
ping -c 3 1.1.1.1         # Cloudflare DNS

# Test traceroute
traceroute google.com

# Test DNS lookup
nslookup google.com
dig google.com
```

---

## 🛠️ Hands-On Practice Lab

### Complete Network Discovery Exercise
```bash
# 1. Document your network topology
# From R1:
echo "=== R1 Interface Status ===" > network_topology.txt
show interface status >> network_topology.txt

# From R2:
echo "=== R2 Interface Status ===" >> network_topology.txt
show interface status >> network_topology.txt

# 2. Test all links systematically
# Create connectivity matrix
for ip in 10.1.1.2 10.1.2.2 10.1.3.2; do
    echo "Testing connectivity to $ip"
    ping -c 1 $ip && echo "$ip: UP" || echo "$ip: DOWN"
done

# 3. Simulate link failures
# Shutdown each interface one by one and test impact
sudo config interface shutdown Ethernet0
echo "Link 1 down - testing remaining connectivity"
ping -c 1 10.1.2.2 && echo "Link 2: Still UP"
ping -c 1 10.1.3.2 && echo "Link 3: Still UP"

# Bring back and test next
sudo config interface startup Ethernet0
sudo config interface shutdown Ethernet4
# Continue testing...
```

### Network Troubleshooting Scenarios
```bash
# Scenario 1: One-way connectivity issue
# From R1 - Configure asymmetric routing
sudo config interface ip remove Ethernet0 10.1.1.1/30
# Test ping from R2 to R1 on this link - should fail

# Scenario 2: Subnet mismatch
# Misconfigure one end
sudo config interface ip add Ethernet0 10.1.1.5/30  # Wrong subnet
ping -c 3 10.1.1.2  # Should fail

# Fix and verify
sudo config interface ip remove Ethernet0 10.1.1.5/30
sudo config interface ip add Ethernet0 10.1.1.1/30
ping -c 3 10.1.1.2  # Should work

# Scenario 3: Interface status issues
sudo config interface shutdown Ethernet0
show interface status Ethernet0  # Shows admin down
sudo config interface startup Ethernet0
```

---

## 🔍 Network Analysis Commands

### SONiC-Specific Commands
```bash
# Show running configuration
show runningconfiguration all

# Show system information  
show system status
show version

# Show interface counters
show interface counters
show interface counters clear  # Reset counters

# Show ARP table
show arp
show ndp  # For IPv6
```

### Linux Network Analysis
```bash
# Detailed interface statistics
cat /proc/net/dev

# Network namespace information
ip netns list

# Socket statistics
ss -tuln

# Network connections
netstat -rn
```

---

## ✅ Learning Objectives Check

After completing this lab, you should be able to:

- [ ] Deploy SONiC containers in Docker environment
- [ ] Create and manage multiple network links between devices
- [ ] Configure IP addresses on SONiC interfaces
- [ ] Test connectivity using ping and traceroute
- [ ] Simulate and troubleshoot link failures
- [ ] Verify external internet connectivity from containers
- [ ] Use both SONiC and Linux commands for network analysis

---

## 🚀 Real-World Applications

### Enterprise Skills Gained
- **Multi-vendor networking**: SONiC runs on multiple hardware platforms
- **Container networking**: Essential for modern data center operations  
- **Link redundancy**: Understanding multiple path connectivity
- **Failure simulation**: Proactive troubleshooting methodologies
- **Network verification**: Systematic connectivity testing approaches

### Career Relevance
- **Cloud networking expertise**: SONiC skills are highly valued
- **Modern data center operations**: Container-based network functions
- **Automation readiness**: Programmatic network management
- **Troubleshooting methodology**: Systematic approach to network issues

**Ready for Task 4?** You now understand container networking, multi-link connectivity, and modern network operating systems!
