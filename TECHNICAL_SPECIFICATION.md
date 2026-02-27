# Technical Specification

## 📊 General Information

**Project Name**: Warehouse Management and Equipment Tracking System
**Project Type**: Diploma Thesis - Full-Stack Management System
**Status**: MVP Ready for Demonstration
**Version**: 1.0.0
**Created Date**: November 2024
**Last Updated**: December 2024

## 🎯 Project Goals and Objectives

### Primary Goal

Development of a modern warehouse operations management system with blockchain integration to ensure transparency and immutability of equipment movement records.

### Objectives

1. **Warehouse Process Automation** - tracking of receipts, expenditures, and inventory.
2. **Equipment Tracking** - full lifecycle management from purchase to disposal.
3. **Blockchain Integration** - immutable log of transfers between employees.
4. **Role-Based Access Control** - access rights differentiation based on user roles.
5. **Notification System** - informing users about critical events.
6. **Reporting and Analytics** - providing data for decision-making.

## 🏛️ System Architecture

### Architectural Approach

**Microservices Architecture** with separation of concerns between services:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Frontend  │    │ Auth Service│    │ Warehouse   │
│   Next.js   │◄──►│     Go      │◄──►│   Service   │
│             │    │             │    │     Go      │
└─────────────┘    └─────────────┘    └─────────────┘
       ▲                   ▲                   ▲
       │                   │                   │
       ▼                   ▼                   ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ Tracking    │    │Notification │    │ Analytics   │
│  Service    │    │  Service    │    │  Service    │
│  Node.js    │    │     Go      │    │     Go      │
└─────────────┘    └─────────────┘    └─────────────┘
       ▲                   ▲
       │                   │
       ▼                   ▼
┌─────────────┐    ┌─────────────┐
│   MongoDB   │    │  RabbitMQ   │
│   Cluster   │    │   Broker    │
└─────────────┘    └─────────────┘
       ▲
       │
       ▼
┌─────────────┐
│  Ethereum   │
│  Blockchain │
│  (Ganache)  │
└─────────────┘
```

### Architecture Principles

- **Single Responsibility** - each service performs one function.
- **Loose Coupling** - weak dependency between services.
- **High Cohesion** - high cohesion within services.
- **Scalability** - ability to scale individual components.
- **Fault Tolerance** - system resilience to failures.
- **Security by Design** - security at all levels.

## 📦 System Components

### Frontend (Next.js 15)

**Technologies**: Next.js 15, React 19, TypeScript 5, Mantine UI 8, Tailwind CSS 4

**Key Features**:

- App Router for optimal performance
- Server-Side Rendering for SEO optimization
- Static Generation for static pages
- Redux Toolkit for state management
- React Query for API data caching
- Responsive design for all devices

**Pages and Functionality**:

- 🏠 **Dashboard** - metrics, analytics, system overview
- 🔐 **Authentication** - login/logout, session management
- 📦 **Warehouse Management** - item CRUD, inventory
- 🔗 **Equipment Tracking** - transfers, blockchain records
- 📋 **Invoices** - creation, approval, document printing
- 👥 **User Management** - role administration
- 📊 **Reports** - analytics, data export

### Backend Services

#### 🔐 Auth Service (Go)

**Purpose**: Centralized authentication and authorization

**Functionality**:

- JWT tokens with automatic refresh
- Role model (Admin, Manager, Operator, Viewer)
- Auto-generation of Ethereum addresses for users
- User action audit
- Password hashing with bcrypt
- Session management

**Database**: `warehouse_auth` (3 collections)

- `users` - system users
- `audit_logs` - action log
- `system_settings` - system settings

#### 📦 Warehouse Service (Go)

**Purpose**: Management of warehouse operations

**Functionality**:

- CRUD operations for warehouse items
- Invoice system (incoming/outgoing)
- Categories and item references
- Automated stock calculation
- Critical stock notifications
- RabbitMQ integration

**Database**: `warehouse_inventory` (6 collections)

- `warehouseitems` - item catalog
- `inventorytransactions` - operation history
- `invoices` - invoices
- `categories` - item categories
- `warehouses` - warehouse reference
- `suppliers` - suppliers

#### 🔗 Tracking Service (Node.js/Express)

**Purpose**: Equipment tracking with blockchain integration

**Functionality**:

- Equipment registration in blockchain
- Transfers between users
- Ethereum smart contracts
- QR codes for identification
- Maintenance schedule
- Web3.js integration

**Database**: `warehouse_tracking` (3 collections)

- `equipments` - equipment registry
- `transfers` - transfer history
- `maintenance_schedules` - maintenance schedule

#### 🔔 Notification Service (Go)

**Purpose**: Real-time notification system

**Functionality**:

- Real-time notifications
- Various notification types
- Email integration (ready)
- Push notifications
- RabbitMQ consumer/producer

#### 📊 Analytics Service (Go)

**Purpose**: Analytics and reporting (basic implementation)

**Functionality**:

- System metrics collection
- Report generation
- Analytics dashboard
- Data export

### Infrastructure Components

#### 🗄️ MongoDB

**Version**: 7.0
**Configuration**: Standalone instance for MVP
**Databases**: 3 separate DBs for data isolation

**Features**:

- Indexes for query optimization
- Data schema validation
- Automatic timestamps
- Connection pooling
- Health checks

#### 🐰 RabbitMQ

**Version**: 3.12
**Purpose**: Asynchronous communication between services

**Queues**:

- `inventory.notifications` - warehouse notifications
- `equipment.transfers` - equipment transfers
- `maintenance.alerts` - maintenance
- `system.audit` - system audit

#### ⛓️ Ethereum Blockchain

**Network**: Ganache (local development)
**Purpose**: Immutable log of equipment transfers

**Smart Contracts**:

- `EquipmentTracking.sol` - main tracking contract
- Events for transfer logging
- Access control for security

## 🛠️ Technology Stack

### Backend

| Technology         | Version | Purpose              |
| ------------------ | ------- | -------------------- |
| **Go**             | 1.21+   | Backend microservices|
| **Gin**            | 1.9     | HTTP web framework   |
| **MongoDB Driver** | 1.12    | Database             |
| **JWT-Go**         | 4.5     | JWT tokens           |
| **Bcrypt**         | latest  | Password hashing     |
| **Validator**      | 10.15   | Data validation      |
| **AMQP**           | 1.9     | RabbitMQ integration |

### Frontend

| Technology        | Version | Purpose               |
| ----------------- | ------- | --------------------- |
| **Next.js**       | 15.0    | React framework       |
| **React**         | 19.0    | UI library            |
| **TypeScript**    | 5.0     | Static typing         |
| **Mantine**       | 8.0     | UI components         |
| **Tailwind CSS**  | 4.0     | CSS framework         |
| **Redux Toolkit** | 2.8     | State management      |
| **React Query**   | 5.76    | Server state          |
| **Axios**         | 1.9     | HTTP client           |

### DevOps & Tools

| Technology         | Version | Purpose          |
| ------------------ | ------- | ---------------- |
| **Docker**         | 20.10+  | Containerization |
| **Docker Compose** | 2.0+    | Orchestration    |
| **MongoDB**        | 7.0     | Database         |
| **RabbitMQ**       | 3.12    | Message broker   |
| **Ganache**        | 7.9     | Ethereum testnet |
| **Swagger**        | 3.0     | API documentation|

## 📊 Project Metrics

### Codebase Size

```
Service              Files    Lines of Code    Languages
─────────────────────────────────────────────────────
Frontend             120+     ~8,000        TS/TSX/CSS
Auth Service         8        ~1,200        Go
Warehouse Service    9        ~1,500        Go
Tracking Service     25+      ~2,000        JS/Sol
Notification Service 6        ~800          Go
Analytics Service    4        ~400          Go
Documentation        8        ~5,000        Markdown
Docker & Scripts     15+      ~500          YAML/Shell
─────────────────────────────────────────────────────
Total               195+      ~19,400       7 languages
```

### Functional Coverage

- ✅ **Authentication**: 100% - Full implementation with JWT
- ✅ **Warehouse Management**: 95% - Main functionality + invoices
- ✅ **Tracking**: 90% - Blockchain integration + QR codes
- ✅ **Notifications**: 85% - Real-time + notification types
- ✅ **Frontend UI**: 95% - All main pages
- 🔄 **Analytics**: 30% - Basic structure
- ✅ **API Documentation**: 100% - Swagger for all services
- ✅ **Containerization**: 100% - Docker Compose ready

### Database

- **Collections**: 12 (100% implemented)
- **Indexes**: 25+ for optimization
- **Demo Data**: 500+ records for testing
- **Validation Schemas**: Full coverage

## 🔒 Security

### Authentication and Authorization

- **JWT tokens** with short lifetime (15 min)
- **Refresh tokens** for automatic renewal (7 days)
- **Role-based model** with 4 access levels
- **Authorization middleware** on all protected endpoints

### Data Protection

- **Bcrypt hashing** for passwords (cost 12)
- **Input validation** at all levels
- **SQL/NoSQL injection** protection via prepared statements
- **XSS protection** via data sanitization
- **CORS settings** for frontend-backend communication

### Audit and Monitoring

- **Audit logging** of all critical operations
- **Error handling** with structured logging
- **Health checks** for all services
- **Rate limiting** (ready for implementation)

## 📈 Performance

### Optimizations

- **Database indexing** for fast queries
- **Connection pooling** for MongoDB
- **React Query caching** for API data
- **Code splitting** in Next.js application
- **Docker multi-stage builds** for image optimization

### Metrics

- **API response time**: < 100ms for basic operations
- **Page load time**: < 2 seconds for all pages
- **Docker startup**: < 3 minutes for full system
- **Memory usage**: Optimized for deployment

## 🧪 Testing

### Test Coverage

- **Unit tests**: Critical backend functions
- **Integration tests**: API endpoints
- **API tests**: Automated scripts
- **Manual tests**: UI flows and user scenarios

### Testing Tools

- **Go testing**: Built-in testing package
- **Jest**: For JavaScript/TypeScript tests
- **Postman-like scripts**: For API testing
- **Docker health checks**: For infrastructure testing

## 📦 Deployment

### Containerization

```yaml
# Docker Compose includes:
services:
  - frontend (Next.js)
  - auth-service (Go)
  - warehouse-service (Go)
  - tracking-service (Node.js)
  - notification-service (Go)
  - mongo (MongoDB)
  - rabbitmq (RabbitMQ)
  - ethereum-node (Ganache)
```

### Automation Scripts

- `start-system.sh` - Full system startup
- `clean-and-rebuild.sh` - Rebuild all components
- `check-status.sh` - Status monitoring
- `demo-data-setup.sh` - Load test data

### Environment Management

- **Development**: Local Docker Compose
- **Production Ready**: Environment variables for configuration
- **Scalability**: Readiness for Kubernetes deployment

## 📚 Documentation

### Documentation Types

1. **README.md** - Project overview and quick start
2. **INSTALLATION.md** - Detailed installation guide
3. **API_REFERENCE.md** - Full API documentation
4. **Architecture Documentation** - Technical architecture
5. **Frontend Architecture** - Frontend implementation details
6. **CONTRIBUTING.md** - Guide for developers
7. **SECURITY.md** - Security policies
8. **CHANGELOG.md** - Change history

### API Documentation

- **Swagger UI** for all backend services
- **OpenAPI 3.0** specifications
- **Postman collections** for testing
- **Code examples** for integration

## 🎯 Demonstration Capabilities

### Ready Scenarios for Demonstration

#### Scenario 1: Warehouse Management

1. Login as Manager
2. View dashboard metrics
3. Add new item
4. Create incoming invoice
5. View updated stock

#### Scenario 2: Equipment Tracking

1. Register new equipment
2. Transfer equipment between users
3. View blockchain record
4. Generate QR code
5. Schedule maintenance

#### Scenario 3: Notification System

1. Setup low stock notifications
2. Automatic notification on critical level
3. Notifications on equipment transfers
4. Email notifications (readiness demo)

### Demo Data

- **10 users** with different roles
- **50+ equipment items** of various categories
- **100+ warehouse transactions**
- **20+ invoices** of different types
- **15+ blockchain transfers**
- **Maintenance schedules**

## 🚀 Scalability and Development

### Architectural Readiness

- **Microservices architecture** allows independent scaling
- **API-first approach** simplifies integration
- **Stateless services** support horizontal scaling
- **Database per service** ensures data isolation

### Development Plans

- **Kubernetes deployment** for production
- **API Gateway** (Nginx/Traefik) for routing
- **Elasticsearch** for advanced search
- **Redis** for caching
- **Monitoring stack** (Prometheus/Grafana)
- **CI/CD pipeline** (GitHub Actions)

## 💼 Business Value

### Solved Problems

1. **Manual accounting** → Automated system
2. **Lack of traceability** → Blockchain records
3. **Siloed data** → Centralized system
4. **Slow reporting** → Real-time analytics
5. **Human errors** → Automatic validation

### System Benefits

- **Increased efficiency** of warehouse operations
- **Reduced losses** of equipment and goods
- **Transparency of operations** via blockchain
- **Fast decision making** based on data
- **Audit and compliance** adherence

## 🏆 Project Achievements

### Technical Achievements

- ✅ **Full-stack microservices architecture**
- ✅ **Blockchain technology integration**
- ✅ **Modern responsive frontend**
- ✅ **Comprehensive API with documentation**
- ✅ **Full system containerization**
- ✅ **Demo data and scenarios**

### Educational Outcomes

- **Practice of modern technologies** - Go, Next.js, Blockchain
- **Architectural thinking** - designing complex systems
- **DevOps skills** - Docker, deployment automation
- **Full development cycle** - from concept to deployment
- **Documentation** - technical project documentation

---

**Conclusion**: The project demonstrates the creation of a full-stack enterprise-level system using modern technologies and architectural approaches. The system is ready for demonstration, further development, and potential commercial use.
