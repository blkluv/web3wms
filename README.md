# 🔗 Web3WMS - Blockchain Warehouse Management System

> **Modern warehouse management and equipment tracking system with blockchain integration**

[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)](https://golang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-43853D?style=flat&logo=node.js&logoColor=white)](https://nodejs.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=flat&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![Ethereum](https://img.shields.io/badge/Ethereum-3C3C3D?style=flat&logo=ethereum&logoColor=white)](https://ethereum.org/)

[Читать на русском](README.ru.md)

## 📋 Project Description

A modern warehouse management system with blockchain integration for transparent equipment tracking. The system is built on a microservices architecture and is designed to automate warehouse processes, equipment accounting, and ensure full traceability of operations.

### 🎯 Key Features

- **🔐 Secure Authentication** with JWT tokens and role-based access control
- **📦 Complete Warehouse Management** with automated accounting and inventory
- **🔗 Blockchain Integration** for an immutable log of equipment transfers
- **📊 Reporting System** with analytics and metrics
- **🔔 Real-time Notifications** via RabbitMQ
- **📱 Modern Web Interface** on Next.js with responsive design
- **🐳 Containerization** with Docker Compose for easy deployment

## 🏗️ System Architecture

### Microservices

| Service                  | Technology              | Port | Description                                |
| ------------------------ | ----------------------- | ---- | ------------------------------------------ |
| **Frontend**             | Next.js 15 + Mantine UI | 80   | User Web Interface                         |
| **Auth Service**         | Go + MongoDB            | 8000 | Authentication and user management         |
| **Warehouse Service**    | Go + MongoDB            | 8001 | Warehouse and inventory management         |
| **Tracking Service**     | Node.js + Express       | 8002 | Equipment tracking + Blockchain            |
| **Notification Service** | Go + RabbitMQ           | 8003 | Notification system                        |
| **Analytics Service**    | Go                      | -    | Analytics (separate deployment)            |

### Infrastructure

- **MongoDB** - Main database (3 separate DBs for services)
- **RabbitMQ** - Message broker for inter-service communication
- **Ethereum (Ganache)** - Local blockchain for MVP
- **Docker & Docker Compose** - Containerization and orchestration

## 🚀 Quick Start

### Prerequisites

- Docker 20.10+ and Docker Compose 2.0+
- Node.js 18+ (for frontend development)
- Go 1.21+ (for backend services development)

### System Launch

1. **Clone the repository**

   ```bash
   git clone https://github.com/oglenyaboss/web3wms.git
   cd warehouse-management-system
   ```

2. **Start the entire system**

   ```bash
   chmod +x start-system.sh
   ./start-system.sh
   ```

3. **Access the system**

   - **Web Interface**: http://localhost

4. **Test Accounts**

   ```
   Administrator:
   Email: admin@warehouse.local
   Password: admin123

   Warehouse Manager:
   Email: manager@warehouse.local
   Password: manager123

   Operator:
   Email: operator@warehouse.local
   Password: operator123
   ```

## 📖 Documentation

### Architectural Documentation

- [📋 General System Architecture](./architecture-documentation.md)
- [🎨 Frontend Architecture](./frontend-architecture-documentation.md)

### Data Collections and Schemas

The system uses 12 MongoDB collections distributed across 3 databases:

- **warehouse_auth** (3 collections) - users, audit, settings
- **warehouse_inventory** (6 collections) - items, transactions, invoices, references
- **warehouse_tracking** (3 collections) - equipment, transfers, maintenance

## 🛠️ Development

### Project Structure

```
📁 mvp/
├── 🎨 frontend/                  # Next.js application
├── 🔐 auth-service/              # Authentication Service (Go)
├── 📦 warehouse-service/         # Warehouse Service (Go)
├── 🔗 tracking-service-express/  # Tracking Service (Node.js)
├── 🔔 notification-service/      # Notification Service (Go)
├── 📊 analytics-service/         # Analytics Service (Go)
├── 🐳 docker-compose.yml         # Container configuration
├── 📋 *.md                       # Documentation
└── 🔧 *.sh                       # Deployment scripts
```

### Local Development

#### Frontend Development

```bash
cd frontend
npm install
npm run dev  # http://localhost:3000
```

#### Backend Development

```bash
# Auth Service
cd auth-service
go mod tidy
go run main.go auth.go model.go mongo.go ethereum.go cleanup.go

# Warehouse Service
cd warehouse-service
go mod tidy
go run main.go model.go mongo.go rabbit.go invoice.go cleanup.go

# Tracking Service
cd tracking-service-express
npm install
npm run dev

# Notification Service
cd notification-service
go mod tidy
go run main.go rabbit.go
```

### Development Scripts

- `./start-system.sh` - Full system startup
- `./clean-and-rebuild.sh` - Rebuild all containers
- `./clean-and-restart.sh` - Clean and restart without rebuild
- `./rebuild-and-restart.sh` - Fast rebuild and restart
- `./check-status.sh` - Check status of all services
- `./demo-data-setup.sh` - Load demo data

## 🔧 Configuration

### Environment Variables

```bash
# Database
MONGO_URI=mongodb://mongo:27017/warehouse_auth

# Security
JWT_SECRET=your_jwt_secret_key_for_mvp

# Services
AUTH_SERVICE_URL=http://auth-service:8000
RABBITMQ_URI=amqp://guest:guest@rabbitmq:5672/

# Blockchain
ETH_NODE_URL=http://ethereum-node:8545
```

### Service Ports

| Service              | Internal Port | External Port |
| -------------------- | ------------- | ------------- |
| Frontend             | 3000          | 80            |
| Auth Service         | 8000          | 8000          |
| Warehouse Service    | 8001          | 8001          |
| Tracking Service     | 8002          | 8002          |
| Notification Service | 8003          | 8003          |
| MongoDB              | 27017         | 27017         |
| RabbitMQ             | 5672          | 5672          |
| RabbitMQ Management  | 15672         | 15672         |
| Ganache              | 8545          | 8545          |

## 🧪 Testing

### API Testing

```bash
# Test all API endpoints
cd tracking-service-express/scripts
chmod +x run-api-test.sh
./run-api-test.sh
```

### Demo Data

```bash
# Load test data
./demo-data-setup.sh
```

### Blockchain Verification

```bash
cd tracking-service-express/scripts
node check-ethereum.js      # Check connection to Ethereum
node check-contract.js      # Check smart contract
node issue-equipment.js     # Test equipment issuance to blockchain
```

## 📊 Functionality

### 🔐 User Management

- User registration and authentication
- Role-based access control (Admin, Manager, Operator, Viewer)
- Automatic Ethereum address creation
- User action audit

### 📦 Warehouse Management

- Equipment catalog with detailed characteristics
- Invoice system (incoming/outgoing)
- Automated inventory and movement tracking
- Critical stock notifications
- Inventory and reporting

### 🔗 Equipment Tracking

- Equipment registration in blockchain
- Immutable log of transfers between employees
- QR codes for quick identification
- Maintenance and repair history
- Geolocation and equipment statuses

### 🔔 Notification System

- Real-time notifications
- Email newsletters (integration ready)
- Push notifications in web interface
- Customizable notification types

### 📊 Reporting and Analytics

- Dashboard with key metrics
- Item movement charts
- Equipment usage reports
- Data export in various formats

## 🔒 Security

- **JWT Authentication** with refresh tokens
- **Role-Based Access Control** with granular permissions
- **Data Validation** at all levels
- **User Action Audit**
- **Protected API Endpoints** with authorization middleware
- **Blockchain Immutability** for critical operations

## 🚀 Deployment

### Production Ready Features

- ✅ Docker containerization of all services
- ✅ Health checks for monitoring
- ✅ Logging and error handling
- ✅ Graceful shutdown mechanisms
- ✅ Environment-based configuration
- ✅ Database migrations and seed data

### Scalability Readiness

- Microservices architecture
- Stateless services
- Horizontal scaling readiness
- API Gateway readiness (Nginx/Traefik)
- Kubernetes manifests (in development)

## 📈 Roadmap

### Current MVP (v1.0)

- ✅ Basic functionality of all services
- ✅ Web interface with main features
- ✅ Blockchain integration for tracking
- ✅ Demo data and tests

### Planned Improvements (v2.0)

- 📱 Mobile app (React Native)
- 🔍 Elasticsearch for search and analytics
- 📊 Advanced analytics and BI
- 🌐 Multi-tenant architecture
- 🔐 OAuth2/OpenID Connect integration
- 📡 IoT sensors for automated tracking

## 👨‍💻 Author

**Lozhkin Lenya** - Diploma Project
📧 Email: oglenyaboss@icloud.com
💼 GitHub: [GitHub](https://github.com/oglenyaboss)
✈️ Telegram: [Telegram](https://t.me/ll_ogl)

## 📄 License

This project is developed as part of a diploma thesis. All rights reserved.

## 🙏 Acknowledgements

- To teachers and supervisor Chichikov Sergey Anatolyevich
- To the open-source developer community
- To authors of used libraries and frameworks

---

⭐ **If you like the project, give it a star!** ⭐

> _This project demonstrates skills in full-stack development of modern web applications using microservices architecture, blockchain technologies, and a modern technology stack._
