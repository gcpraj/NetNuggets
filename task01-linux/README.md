# Task 1: Linux Basics for Network Engineers 🐧

## Why Linux Dominates Enterprise Networking

Linux powers **96.3% of the world's top 1 million servers** and runs on virtually every network device you'll encounter in enterprise environments:

- **Network Operating Systems**: Cisco IOS XE, Juniper Junos, Arista EOS all run on Linux
- **Cloud Infrastructure**: AWS, Google Cloud, Azure - over 90% Linux-based
- **Network Management**: Nagios, Zabbix, SolarWinds collectors run on Linux
- **Automation Platforms**: Ansible, Puppet, Chef operate primarily on Linux
- **Container Orchestration**: Kubernetes clusters are 100% Linux in production

**Bottom Line**: Mastering Linux isn't optional for network engineers - it's essential. Every major networking vendor, cloud provider, and enterprise runs on Linux infrastructure.

---

## Step 1: Environment Setup

### Windows Users - Enable WSL
```
# Run in PowerShell as Administrator
wsl --install
# Restart computer after installation
```

### Mac Users
You're good to go! Open Terminal application.

### Linux Users
You're already set! This is the recommended environment.

---

## Step 2: Basic Navigation Commands

### Directory Navigation
```bash
# See where you are
pwd

# List files and directories
ls
ls -la        # Detailed view
ls -lh        # Human readable sizes

# Create directories
mkdir practice
mkdir -p projects/networking/configs

# Change directories and perform 'pwd' at each level
cd practice
cd ..         # Go up one level
cd ~          # Go to home directory
cd -          # Go back to previous directory
```

---

## Step 3: vi Editor Mastery

### Creating and Editing Files with vi
```bash
# Create new file
vi router.conf

# Vi modes:
# ESC - Command mode (default)
# i   - Insert mode (start typing)
# :w  - Save file
# :q  - Quit
# :wq - Save and quit
# :q! - Quit without saving

# Navigation in command mode:
# h,j,k,l - Left, down, up, right
# 0 - Beginning of line
# $ - End of line
```

### Practice with vi
```bash
# Create a router configuration
vi router1.conf

# In insert mode (press 'i'), type:
hostname Router1
interface GigabitEthernet0/1
ip address 192.168.1.1 255.255.255.0
no shutdown

# Save and exit: ESC, then :wq

# Reopen and modify
vi router1.conf

# Add new lines at the end:
interface GigabitEthernet0/2  
ip address 192.168.2.1 255.255.255.0
no shutdown

# Save and exit: ESC, then :wq
```

---

## Step 4: File Utilities

### cat - View Files
```bash
# Display file contents
cat router1.conf

# Display with line numbers
cat -n router1.conf

# Combine multiple files
cat router1.conf router2.conf
```

### grep - Search Text
```bash
# Search for specific text
grep "interface" router1.conf
grep "192.168" router1.conf

# Case-insensitive search
grep -i "INTERFACE" router1.conf

# Show line numbers
grep -n "ip address" router1.conf
```

---

## 🛠️ Hands-On Practice

### Complete Practice Session
```bash
# 1. Setup practice environment
mkdir ~/network-lab
cd ~/network-lab

# 2. Create router configurations using vi
vi router1.conf
# Add this content:
hostname R1
interface eth0
ip address 10.1.1.1/24
interface eth1  
ip address 10.1.2.1/24

vi router2.conf
# Add this content:
hostname R2
interface eth0
ip address 10.1.1.2/24
interface eth1
ip address 10.1.3.1/24

vi switch.conf
# Add this content:
hostname SW1
vlan 10
name SALES
vlan 20
name MARKETING

# 3. Practice navigation and viewing
ls -la
cat router1.conf
cat router2.conf switch.conf

# 4. Search through configurations
grep "10.1.1" *.conf
grep -n "interface" router1.conf
grep "vlan" switch.conf

# 5. Modify existing files
vi router1.conf
# Add: description "Main Router"
# Save and exit

# 6. Verify changes
cat router1.conf
grep "description" router1.conf
```
