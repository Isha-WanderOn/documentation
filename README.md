# WanderOn Finance Management System (FMS) Backend

A comprehensive finance management system designed for WanderOn travel company to handle financial transactions, payment collection, receipt generation, and customer communication automation.

## Project Overview

The FMS Backend is a robust financial management system that handles:

- **Multi-bank transaction processing** (HDFC, HSBC, RBL, Razorpay)
- **Payment Marking** against booking ids.
- **GST-compliant receipt generation** with PDF creation
- **Credit note redeemption system**
- **Ticket system** for correction
- **Payment collection tracking** with reminders
- **Automated communication** (email/Whatsapp SMS reminders)
- **Background job processing** for PDF generation and email delivery

## Project Structure Overview

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

# Getting Started

## Prerequisites

- Node.js 18.x or higher
- Yarn package manager
- MongoDB 5.x or higher
- Redis 6.x or higher

## Installation

```bash
# Clone the repository
git clone <repository-url>
cd fms-backend

# Install dependencies
yarn install

#create .env.development file

# Start development server
yarn run dev
```

## Environment Configuration

Create `.env.development` file with required variables:

```bash
NODE_ENV=development
FMS_DB_URI=mongodb://localhost:27017/fms_development
BOOKINGS_DB_URI=mongodb://localhost:27017/bookings_development
REDIS_HOST=localhost
REDIS_PORT=6379
SESSION_SECRET=your-session-secret
SESSION_TOKEN_SECRET=your-jwt-secret
RAZORPAY_KEY_ID=your-razorpay-key
PDFCROWD_USERNAME=your-pdfcrowd-username
PDFCROWD_API_KEY=your-pdfcrowd-api-key
MAIL_EMAIL=your-email@gmail.com
MAIL_PASS=your-app-password
AWS_ACCESS_KEY_ID=your-aws-access-key
AWS_SECRET_ACCESS_KEY=your-aws-secret-key
AWS_REGION=your-aws-region
AWS_S3_BUCKET=your-s3-bucket-name
EASYCRON_API_KEY=your-easycron-api-key
DOUBLE_TICK_API_KEY=your-doubletick-api-key

# Google Sheets Integration
GOOGLE_SERVICE_ACCOUNT_EMAIL=your-service-account@project.iam.gserviceaccount.com
GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nYOUR_PRIVATE_KEY\n-----END PRIVATE KEY-----\n"

# Internal API Configuration
INTERNAL_API_KEY=your-internal-api-key
INTERNAL_API_EMAIL=internal@wanderon.in

# Application URLs
FRONTEND_URL=http://localhost:3000
BACKEND_BASE_URL=http://localhost:8080
POSTMAN_URL=your-postman-workspace-url
POSTMAN_API_KEY=your-postman-api-key
POSTMAN_COLLECTION_ID=your-collection-id

# Request Configuration
REQUEST_LIMIT=100mb
LOG_LEVEL=info
```
# Technology Stack

## Backend Framework

- **Runtime**: Node.js 18
- **Language**: TypeScript 4.6
- **Framework**: Express.js
- **Validation**: Zod schema validation
- **Authentication**: JWT with Redis session storage

## Databases

- **Primary Database**: MongoDB (FMS data)
- **Secondary Database**: MongoDB (Bookings data)
- **Cache & Sessions**: Redis
- **File Storage**: AWS S3

## External Integrations

- **Payment Gateways**: Razorpay Webhook, Spreadsheets Multiple Banks (HDFC, RBL, HSBC)
- **Google Sheets API**: Service Account authentication for bank data import
- **PDF Generation**: PDFCrowd service for receipt/invoice PDF creation
- **Email Service**: Nodemailer + Gmail/SMTP
- **WhatsApp API**: DoubleTick for customer notifications
- **Cron Scheduling**: EasyCron for payment reminder automation

## Background Processing

- **Background Jobs**: Receipt generation, PDF creation, email delivery

## Development Tools

- **Package Manager**: Yarn
- **Code Quality**: ESLint + Prettier
- **Logging**: Pino structured logging with rotation
- **Type Checking**: TypeScript strict mode

## Other Documentations

- [System Architecture](docs/system-architecture.md)
- [Background Job Processing](docs/background-jobs.md)
- [Event-Driven Architecture](docs/event-architecture.md)
- [Multi-Database Architecture](docs/database.md)
- [External Integrations](docs/external-integrations.md)
- [Cron Jobs & Automation](docs/cron-automation.md)
- [API Endpoints](docs/api-endpoints.md)
- [API Client Security](docs/api-security.md)
- [Email Template System](docs/email-templates.md)


## Support Document

- **User Manual**: [fms end user manual](https://docs.google.com/document/d/141sh5Ca35eJvW42oneFnEJYT6xm6UmxDpWOQibs6ZJ0/edit?usp=sharing)