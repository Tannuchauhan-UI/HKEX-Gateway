# HKEX Trading Platform

A full-stack trading platform for Hong Kong Exchange (HKEX) stocks with real-time market data and order management.

## Features

- Real-time market data for 20+ HKEX stocks
- User authentication and authorization
- Order management (place, modify, cancel orders)
- Real-time order status updates
- Market and limit orders support
- Responsive web interface

## Project Structure

```
├── backend/               # Node.js Express backend
│   ├── src/              # Source files
│   │   ├── server.js     # Main server file
│   │   └── ...          
├── frontend/             # React frontend
│   ├── src/             
│   │   ├── components/   # React components
│   │   ├── pages/       # Page components
│   │   └── ...
├── database/             # Database scripts and migrations
├── config/              # Configuration files
└── README.md           # This file
```

## Prerequisites

- Node.js v14 or higher
- MySQL 8.0 or higher
- npm or yarn package manager

## Setup Instructions

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd hkex-trading-platform
   ```

2. Backend Setup:
   ```bash
   cd backend
   npm install
   cp .env.example .env  # Create and configure your .env file
   npm run dev
   ```

3. Frontend Setup:
   ```bash
   cd frontend
   npm install
   PORT=3001 npm start
   ```

4. Database Setup:
   - Create a MySQL database
   - Configure the database connection in backend/.env
   - Run database migrations

## Environment Variables

### Backend (.env)
```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password_here
DB_NAME=trading_platform
JWT_SECRET=your_secure_jwt_secret_here
PORT=5001
```

### Frontend (.env)
```
PORT=3001
REACT_APP_API_URL=http://localhost:5001
```

## Available Scripts

In the backend directory:
- `npm run dev`: Start development server with hot reload
- `npm start`: Start production server
- `npm test`: Run tests

In the frontend directory:
- `npm start`: Start development server
- `npm build`: Build for production
- `npm test`: Run tests

## API Endpoints

### Authentication
- POST /api/auth/register - Register new user
- POST /api/auth/login - Login user

### Market Data
- GET /api/stocks - Get all stocks
- GET /api/stocks/:symbol - Get specific stock

### Orders
- GET /api/orders - Get user orders
- POST /api/orders - Place new order
- DELETE /api/orders/:orderId - Cancel order

