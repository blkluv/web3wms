# Tracking Service Express

Equipment tracking service with blockchain integration using Express.js.

[Читать на русском](README.ru.md)

## Features

- Equipment registration and management
- Tracking equipment transfers between employees
- Integration with Ethereum blockchain for reliable transfer history storage
- RESTful API for interaction with other services

## Installation and Launch

### Prerequisites

- Node.js v14+
- MongoDB
- Ethereum-compatible network (optional)

### Installation

1. Clone repository
2. Install dependencies:
   ```
   npm install
   ```
3. Create `.env` file based on `.env.example`
4. Start the service:
   ```
   npm start
   ```
   Or in development mode:
   ```
   npm run dev
   ```

## API

The service provides an API for managing equipment and its transfers:

### Equipment Registration and Management

- `POST /equipment` - Register new equipment
- `GET /equipment/:id` - Get equipment information
- `GET /equipment` - Equipment list (with filtering)
- `PUT /equipment/:id` - Update equipment information
- `DELETE /equipment/:id` - Delete equipment

### Equipment Transfer

- `POST /transfer` - Transfer equipment
- `GET /transfer/history/:equipment_id` - Equipment transfer history

### Blockchain Integration

- `GET /blockchain/history/:equipment_id` - Equipment transfer history from blockchain
- `POST /blockchain/register` - Register existing equipment in blockchain

### Service Status

- `GET /ping` - Check service availability
- `GET /operations/recent` - Get recent operations

## Blockchain Integration

To use blockchain integration:

1. Deploy `EquipmentTracking.sol` smart contract to Ethereum network
2. Place contract ABI in `/contracts` directory
3. Specify contract address and private key in `.env`

## Project Structure

```
tracking-service-express/
├── config/                  # Configuration files
│   ├── db.js                # MongoDB connection settings
│   └── blockchain.js        # Ethereum connection settings
├── controllers/             # Request handling controllers
├── middleware/              # Express middlewares
├── models/                  # Mongoose models
├── routes/                  # API routes
├── services/                # Business logic
├── utils/                   # Utilities
├── contracts/               # Directory for ABI and Ethereum contracts
├── app.js                   # Main application file
├── server.js                # Server startup file
└── package.json             # Dependencies and scripts
```
