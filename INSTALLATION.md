# Installation and Launch Guide

## 🎯 Document Goal

This guide will help you deploy and launch the warehouse management and equipment tracking system on your local computer or server.

## 📋 System Requirements

### Minimum Requirements

- **OS**: Windows 10+, macOS 10.15+, Ubuntu 18.04+ or other Linux distribution
- **RAM**: 8 GB (16 GB recommended)
- **Disk Space**: 10 GB free space
- **CPU**: Intel Core i5 or AMD Ryzen 5 (2+ cores)

### Required Software

#### Mandatory for Launch

- **Docker Desktop 4.0+** - [Download](https://www.docker.com/products/docker-desktop)
- **Docker Compose 2.0+** (usually included in Docker Desktop)
- **Git** - [Download](https://git-scm.com/downloads)

#### For Development (Optional)

- **Node.js 18+** - [Download](https://nodejs.org/en/download/)
- **Go 1.21+** - [Download](https://golang.org/dl/)
- **Visual Studio Code** or other IDE

## 🚀 Step-by-Step Installation

### Step 1: Install Docker

#### Windows

1. Download Docker Desktop from the official website
2. Run the installer and follow instructions
3. Restart computer after installation
4. Start Docker Desktop and wait for full load

#### macOS

1. Download Docker Desktop for Mac
2. Drag Docker to Applications folder
3. Start Docker from Applications folder
4. Grant necessary system permissions

#### Linux (Ubuntu/Debian)

```bash
# Update packages
sudo apt update

# Install required packages
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Add official Docker GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add Docker repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Add user to docker group
sudo usermod -aG docker $USER

# Log out and back in or run:
newgrp docker
```

### Step 2: Verify Installation

```bash
# Check Docker
docker --version
# Expected output: Docker version 24.0.x

# Check Docker Compose
docker-compose --version
# Expected output: Docker Compose version 2.x.x
```

### Step 3: Clone Project

```bash
# Clone repository
git clone https://github.com/yourusername/warehouse-management-system.git

# Navigate to project directory
cd warehouse-management-system
```

### Step 4: Environment Setup (Optional)

Create `.env` file for customization:

```bash
# Create environment file
cp .env.example .env
```

Example `.env` content:

```env
# Databases
MONGO_URI=mongodb://mongo:27017
POSTGRES_URI=postgresql://postgres:password@postgres:5432/warehouse

# Security
JWT_SECRET=your_super_secret_jwt_key_change_in_production
ENCRYPTION_KEY=your_encryption_key_32_characters_long

# External APIs
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password

# Feature flags
ENABLE_BLOCKCHAIN=true
ENABLE_NOTIFICATIONS=true
ENABLE_ANALYTICS=false
```

## 🚀 Launching the System

### Automatic Launch (Recommended)

```bash
# Make script executable
chmod +x start-system.sh

# Start the entire system
./start-system.sh
```

This script will automatically:

- Create necessary Docker networks
- Start all services in the correct order
- Wait for database readiness
- Load demo data
- Display available URLs

### Manual Launch

```bash
# Create Docker network
docker network create warehouse-network

# Start infrastructure services
docker-compose up -d mongo rabbitmq ethereum-node

# Wait for databases (30-60 seconds)
sleep 60

# Start backend services
docker-compose up -d auth-service warehouse-service tracking-service notification-service

# Wait for API (30 seconds)
sleep 30

# Start frontend
docker-compose up -d frontend

# View logs of all services
docker-compose logs -f
```

### Check Status

```bash
# Use built-in script
./check-status.sh

# Or check manually
docker-compose ps
```

Expected output:

```
Name                    Command               State                    Ports
-------------------------------------------------------------------------------------------
mvp_auth-service_1      ./auth-service                Up      0.0.0.0:8000->8000/tcp
mvp_frontend_1          docker-entrypoint.sh npm ...  Up      0.0.0.0:80->3000/tcp
mvp_mongo_1             docker-entrypoint.sh mongod   Up      0.0.0.0:27017->27017/tcp
mvp_notification-service_1  ./notification-service    Up      0.0.0.0:8003->8003/tcp
mvp_rabbitmq_1          docker-entrypoint.sh rabbitmq Up      4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp
mvp_tracking-service_1  docker-entrypoint.sh npm ...  Up      0.0.0.0:8002->8002/tcp
mvp_warehouse-service_1 ./warehouse-service           Up      0.0.0.0:8001->8001/tcp
```

## 🌐 Accessing the System

### Main URLs

| Service              | URL                   | Description            |
| -------------------- | --------------------- | ---------------------- |
| **Web Interface**    | http://localhost      | Main application       |
| **Auth API**         | http://localhost:8000 | Authentication API     |
| **Warehouse API**    | http://localhost:8001 | Warehouse API          |
| **Tracking API**     | http://localhost:8002 | Tracking API           |
| **Notification API** | http://localhost:8003 | Notification API       |

### API Documentation (Swagger)

| Service              | Swagger URL                     |
| -------------------- | ------------------------------- |
| Auth Service         | http://localhost:8000/swagger/  |
| Warehouse Service    | http://localhost:8001/swagger/  |
| Tracking Service     | http://localhost:8002/api-docs/ |
| Notification Service | http://localhost:8003/swagger/  |

### Administrative Panels

| Service                 | URL                       | Login/Password |
| ----------------------- | ------------------------- | -------------- |
| **RabbitMQ Management** | http://localhost:15672    | guest/guest    |
| **MongoDB**             | mongodb://localhost:27017 | -              |

### Test Accounts

```
👑 Administrator:
Email: admin@warehouse.local
Password: admin123

👨‍💼 Warehouse Manager:
Email: manager@warehouse.local
Password: manager123

👷 Warehouse Operator:
Email: operator@warehouse.local
Password: operator123

👁️ Viewer:
Email: viewer@warehouse.local
Password: viewer123
```

## 🧪 Loading Test Data

### Automatic Loading

```bash
# Load demo data
./demo-data-setup.sh
```

### Manual Loading

```bash
# Load data via Node.js script
cd tracking-service-express
node demo-data-script.js

# Or via API calls
curl -X POST http://localhost:8000/api/demo-data
curl -X POST http://localhost:8001/api/demo-data
curl -X POST http://localhost:8002/api/demo-data
```

### What Test Data Includes

- **10 users** with different roles
- **50+ equipment items** of various categories
- **100+ warehouse transactions**
- **20+ invoices** (incoming and outgoing)
- **15+ equipment transfers** with blockchain records
- **Maintenance schedules** for equipment
- **Categories and references**

## 🔧 Troubleshooting

### Problem: Ports Occupied

**Symptoms**: Errors like "port 8000 already in use"

**Solution**:

```bash
# Check which processes are using ports
netstat -tulpn | grep :8000
# or on macOS:
lsof -i :8000

# Kill conflicting processes
sudo kill -9 <PID>

# Or change ports in docker-compose.yml
```

### Problem: Insufficient Memory

**Symptoms**: Containers crash with OOMKilled

**Solution**:

```bash
# Increase memory for Docker Desktop (in settings)
# Or stop unnecessary applications

# Check memory usage
docker stats

# Clean unused images
docker system prune -a
```

### Problem: Database Not Ready

**Symptoms**: Connection errors to MongoDB

**Solution**:

```bash
# Check MongoDB status
docker-compose logs mongo

# Restart only MongoDB
docker-compose restart mongo

# Wait for readiness (can take 1-2 minutes)
docker-compose logs -f mongo | grep "waiting for connections"
```

### Problem: Frontend Not Loading

**Symptoms**: "This site can't be reached" on http://localhost

**Solution**:

```bash
# Check frontend container status
docker-compose logs frontend

# Rebuild frontend
docker-compose build frontend
docker-compose up -d frontend

# Clear browser cache (Ctrl+F5)
```

### Problem: API Unavailable

**Symptoms**: 502/503 errors when accessing API

**Solution**:

```bash
# Check status of all services
./check-status.sh

# Restart problematic service
docker-compose restart auth-service

# Check logs
docker-compose logs -f auth-service
```

## 🔄 System Management

### Stopping the System

```bash
# Stop all services
docker-compose down

# Stop and remove volumes (WARNING: deletes all data)
docker-compose down -v

# Stop specific service
docker-compose stop frontend
```

### Restarting Services

```bash
# Full restart
./clean-and-restart.sh

# Restart without rebuild
docker-compose restart

# Restart specific service
docker-compose restart warehouse-service
```

### Updating the System

```bash
# Pull latest changes
git pull origin main

# Rebuild and restart
./clean-and-rebuild.sh
```

### Viewing Logs

```bash
# Logs of all services
docker-compose logs -f

# Logs of specific service
docker-compose logs -f auth-service

# Logs with time limit
docker-compose logs --since="2h" frontend
```

## 📊 Monitoring and Performance

### Resource Check

```bash
# Container resource usage
docker stats

# Disk space
docker system df

# Image sizes
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

### System Cleanup

```bash
# Clean unused resources
docker system prune

# Full cleanup (WARNING!)
docker system prune -a --volumes
```

## 🆘 Getting Help

### Checklist

- [ ] Docker is running correctly
- [ ] All necessary ports are free
- [ ] Sufficient free memory (8+ GB)
- [ ] Internet connection active (for downloading images)
- [ ] Antivirus not blocking Docker

### Collecting Diagnostic Info

```bash
# System info
docker version
docker-compose version
docker info

# Service status
docker-compose ps
./check-status.sh

# All service logs
docker-compose logs > system-logs.txt
```

### Support Contacts

📧 **Email**: your.email@example.com  
🐛 **Issues**: [GitHub Issues](https://github.com/yourusername/warehouse-management-system/issues)  
📖 **Documentation**: [Wiki](https://github.com/yourusername/warehouse-management-system/wiki)

---

## ✅ Installation Verification

After starting the system, verify:

1. **Web Interface accessible**: http://localhost
2. **Successful login**: using admin@warehouse.local / admin123
3. **API responding**: http://localhost:8000/health
4. **Data loaded**: test items and equipment visible
5. **Notifications working**: appearing in interface

🎉 **Congratulations! System successfully installed and ready to use!**
