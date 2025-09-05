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