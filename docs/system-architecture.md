# System Architecture

## Layered Architecture

The FMS backend follows a **three-tier layered architecture** that promotes separation of concerns and maintainability:

### 1. API Layer (Presentation Tier)

- **Routes**: Express.js route handlers that define API endpoints
- **Middlewares**: Authentication, CORS, validation, error handling
- **Validators**: Zod schema validation for request/response data
- **Controllers**: Handle HTTP requests and coordinate with services

### 2. Service Layer (Business Logic Tier)

- **Financial Services**: Bank transactions, payment collections, receipts
- **Communication Services**: Email, SMS, notifications
- **Integration Services**: External APIs, third-party integrations
- **Business Logic**: Core application logic and workflows

### 3. Data Layer (Data Access Tier)

- **DAOs (Data Access Objects)**: Repository pattern for database operations
- **Schemas**: Mongoose schemas defining data models
- **Cache**: Redis for session storage and caching
- **Database Connections**: MongoDB connections for FMS and Bookings data

## Project Structure

```
server/
├── api/ # API layer
│ ├── routes/ # Express route handlers
│ ├── middlewares/ # Custom middleware
│ └── validators/ # Request validation schemas
├── common/ # Shared utilities
│ ├── helpers/ # Business logic utilities
│ ├── interfaces/ # TypeScript interfaces
│ ├── mailTemplates/ # Email templates
│ └── EventStore.ts # Event definitions
├── database/ # Data layer
│ ├── dao/ # Data Access Objects
│ └── schemas/ # Mongoose schemas
├── services/ # Business logic layer
└── docs/ # Documentation
README.md                     # Main documentation with navigation
```