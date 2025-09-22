# Task 2: Docker Fundamentals 

## Why Docker is Critical in Modern Networking

Docker has revolutionized how network engineers work in enterprise environments:

### **Enterprise Statistics**
- **87% of enterprises** use containers in production (CNCF Survey 2024)
- **Network automation tools** like Ansible, Terraform run in containers
- **Microservices architecture** powers modern network management platforms
- **CI/CD pipelines** for network configuration deployment use Docker

---

## Step 1: Docker Installation

### Windows Users
```powershell
# Install Docker Desktop for Windows
# Download from: https://docker.com/products/docker-desktop

# After installation, enable WSL 2 backend
# Restart computer and verify installation
docker --version
docker run hello-world
```

**Windows Notes:**
- Requires Windows 10/11 Pro or Enterprise
- Uses WSL 2 backend for better performance
- Docker Desktop provides GUI management

### Mac Users
```bash
# Install Docker Desktop for Mac
# Download from: https://docker.com/products/docker-desktop

# Alternative: Using Homebrew
brew install --cask docker

# Verify installation
docker --version
docker run hello-world
```

**Mac Notes:**
- Works on both Intel and Apple Silicon Macs
- Docker Desktop includes Kubernetes
- Native performance with Apple Silicon optimization

### Linux Users
```bash
# Ubuntu/Debian installation
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Add user to docker group (avoid sudo)
sudo usermod -aG docker $USER
# Logout and login again for group changes

# Start Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Verify installation
docker --version
docker run hello-world
```

**Linux Notes:**
- Most efficient Docker performance
- Native container runtime
- No virtualization overhead

---

## Step 2: Bring Up Ubuntu Container

### Basic Ubuntu Container
```bash
# Pull Ubuntu image
docker pull ubuntu:latest

# Run interactive Ubuntu container
docker run -it --name my-ubuntu ubuntu:latest

# You're now inside Ubuntu container!
# Try these commands:
cat /etc/os-release
whoami
pwd
ls -la
```

### Looking Ahead: SONiC Containers for Network Engineering

While we're learning Docker fundamentals with Ubuntu containers, our next major goal is to work with **SONiC (Software for Open Networking in the Cloud)** containers. SONiC is Microsoft's open-source network operating system that powers major cloud infrastructures.

**Why SONiC Containers Matter:**
- **Router Configuration**: Configure virtual routers and switches in containers
- **Configuration Testing**: Validate network configurations in isolated environments
- **Lab Environments**: Create complex network topologies without physical hardware
- **Automation Testing**: Test network automation scripts safely before production deployment

**What We'll Accomplish:**
In the upcoming Docker Networking task, you'll use SONiC containers to:
- Spin up multiple virtual network devices
- Configure interfaces, routing protocols, and network policies
- Test connectivity between virtual routers
- Validate configuration changes before applying to production networks
- Simulate network failures and recovery scenarios

This Docker foundation you're building now directly enables advanced network engineering workflows with containerized network operating systems.

---
```


## Step 3: Essential Docker Commands

### Container Management
```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop container
docker stop my-ubuntu

# Start stopped container
docker start my-ubuntu

# Attach to running container
docker attach my-ubuntu

# Execute command in running container
docker exec -it my-ubuntu bash

# Remove container
docker rm my-ubuntu
```

### Image Management
```bash
# List downloaded images
docker images

# Remove image
docker rmi ubuntu:latest

# Pull specific Ubuntu version
docker pull ubuntu:20.04

# Search for images
docker search nginx
```

---

## Step 4: Monitor Docker Resource Usage

### Check Container Resource Usage
```bash
# View real-time resource usage for all containers
docker stats

# View resource usage for specific container
docker stats my-ubuntu

# View resource usage once (no streaming)
docker stats --no-stream

# View resource usage with specific format
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}"
```

### Check System-Wide Docker Usage
```bash
# View Docker system information
docker system df

# Detailed breakdown of space usage
docker system df -v

# View Docker system events (real-time)
docker system events

# Show Docker system information
docker info
```

### Resource Limits and Monitoring
```bash
# Run container with memory limit (512MB)
docker run -it --memory="512m" --name limited-ubuntu ubuntu:latest

# Run container with CPU limit (0.5 CPU cores)
docker run -it --cpus="0.5" --name cpu-limited ubuntu:latest

# Run container with both memory and CPU limits
docker run -it --memory="256m" --cpus="0.25" --name resource-limited ubuntu:latest

# Monitor the limited container
docker stats resource-limited
```

### Container Process Monitoring
```bash
# View processes running inside container
docker top my-ubuntu

# View detailed container information (including resource usage)
docker inspect my-ubuntu

# Check container logs
docker logs my-ubuntu

# Follow container logs in real-time
docker logs -f my-ubuntu
```

---

## 🔄 Next Steps

### If This Is Your First Linux Machine
If you skipped **task01-Linux** because you don't have a Linux environment, now is the perfect time! Your Ubuntu container is a full Linux environment. 

```bash
# Start your Ubuntu container
docker run -it --name linux-practice ubuntu:latest

# Now go back and complete task01-Linux inside this container
# Practice all the commands: pwd, ls, cd, mkdir, vi, cat, grep
# This container is your safe Linux playground!
```

### Deepen Your Understanding
Before moving to Task 3, invest time in understanding containers:

**Watch These YouTube Videos:**
- "Docker in 100 Seconds" by Fireship
- "Docker Tutorial for Beginners" by TechWorld with Nana  
- "Containers vs Virtual Machines" by IBM Technology
- "Why Use Docker? The Benefits Explained" by NetworkChuck

**Key Concepts to Understand:**
- What problems do containers solve?
- How are containers different from virtual machines?
- Why do enterprises prefer containers for networking tools?
- How does container networking work?

Take your time with this - understanding containers deeply will make you much more valuable as a network engineer.

---
