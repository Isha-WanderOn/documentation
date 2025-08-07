# WanderOn Finance Management System (FMS) Backend

A comprehensive finance management system designed for WanderOn travel company to handle financial transactions, payment collection, receipt generation, and customer communication automation.

## 📋 Table of Contents

- [Project Overview](#project-overview)

- [Technology Stack](#technology-stack)

- [System Architecture](#system-architecture)

- [Getting Started](#getting-started)

- [Background Job Processing](#background-job-processing)

- [Event-Driven Architecture](#event-driven-architecture)

- [Multi-Database Architecture](#multi-database-architecture)

- [External Integrations](#external-integrations)

- [Cron Jobs & Automation](#cron-jobs--automation)

- [API Endpoints](#api-endpoints)

- [API Client Security](#api-client-security)

- [Database Schema](#database-schema)

- [Email Template System](#email-template-system)
- [Support Document](#support-document)

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

## Technology Stack

### Backend Framework

- **Runtime**: Node.js 18

- **Language**: TypeScript 4.6

- **Framework**: Express.js

- **Validation**: Zod schema validation

- **Authentication**: JWT with Redis session storage

### Databases

- **Primary Database**: MongoDB (FMS data)

- **Secondary Database**: MongoDB (Bookings data)

- **Cache & Sessions**: Redis

- **File Storage**: AWS S3

### External Integrations

- **Payment Gateways**: Razorpay Webhook, Spreadsheets Multiple Banks (HDFC, RBL, HSBC)

- **Google Sheets API**: Service Account authentication for bank data import

- **PDF Generation**: PDFCrowd service for receipt/invoice PDF creation

- **Email Service**: Nodemailer + Gmail/SMTP

- **WhatsApp API**: DoubleTick for customer notifications

- **Cron Scheduling**: EasyCron for payment reminder automation

### Background Processing

- **Background Jobs**: Receipt generation, PDF creation, email delivery

### Development Tools

- **Package Manager**: Yarn

- **Code Quality**: ESLint + Prettier

- **Logging**: Pino structured logging with rotation

- **Type Checking**: TypeScript strict mode

## System Architecture

### Layered Architecture

The FMS backend follows a **three-tier layered architecture** that promotes separation of concerns and maintainability:

#### 1. **API Layer (Presentation Tier)**

- **Routes**: Express.js route handlers that define API endpoints

- **Middlewares**: Authentication, CORS, validation, error handling

- **Validators**: Zod schema validation for request/response data

- **Controllers**: Handle HTTP requests and coordinate with services

#### 2. **Service Layer (Business Logic Tier)**

- **Financial Services**: Bank transactions, payment collections, receipts

- **Communication Services**: Email, SMS, notifications

- **Integration Services**: External APIs, third-party integrations

- **Business Logic**: Core application logic and workflows

#### 3. **Data Layer (Data Access Tier)**

- **DAOs (Data Access Objects)**: Repository pattern for database operations

- **Schemas**: Mongoose schemas defining data models

- **Cache**: Redis for session storage and caching

- **Database Connections**: MongoDB connections for FMS and Bookings data

### Project Structure

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
	└── services/ # Business logic layer
```

## Getting Started

### Prerequisites

- Node.js 18.x or higher

- Yarn package manager

- MongoDB 5.x or higher

- Redis 6.x or higher

### Installation

```bash
	# Clone the repository
	git  clone <repository-url>
	cd  fms-backend

	# Install dependencies
	yarn  install

	#create .env.development file

	# Start development server
	yarn  run  dev
```

### Environment Configuration

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

## Background Job Processing

### Queue Architecture

The FMS system uses **Mongo collection as an queue** for background processing:

#### Queue Types:

- **Receipt Queue** - Processes sales receipt generation jobs

- **Invoice Queue** - Handles invoice generation tasks

- **PDF Queue** - Manages PDF creation via PDFCrowd API

#### Smart Processor Features:

- Automatic restart capabilities

- Job retry mechanisms with configurable limits

- Queue monitoring and status tracking

- Parallel processing with controlled concurrency

- Failed job recovery and analysis

#### Queue Flow Example:

```
Transaction Marked → Queue Job Created → PDF Generation → S3 Upload → Email Notification → Job Complete
```

#### Queue Management:

- **Graceful shutdown** with job completion

### Queue Consumer Services:

- `ReceiptQueueConsumerService` - Smart processor for receipt generation

- `InvoiceReceiptQueueConsumerService` - Invoice processing automation

- `PdfQueueConsumerService` - PDF generation job handler

## Event-Driven Architecture

### Custom Event Emitter

The system uses a custom event emitter for loose coupling between services:

#### Key Events:

- `SEND_MAIL` - Email sending events

- `API_CLIENT_*` - API client management events

- `BOOKING_MARKED_TO_TRANSACTION` - Transaction marking events

- `NEW_RECEIPT_JOB` - Receipt generation events

- `CREDIT_NOTES_MIGRATION` - Migration events

- `UPDATE_PAYMENT_COLLECTIONS_SCRIPT` - Payment update events

#### Event Flow Example:

```typescript
// Emit event
eventEmitter.emit(EventStore.SEND_MAIL, templateName, to, data);
// Listen for event
eventEmitter.on(EventStore.SEND_MAIL, async (template, email, data) => {
  await mailer.createMailAndSend(template, email, data);
});
```

## Multi-Database Architecture

### Database Separation:

- **FMS Database** - Financial data, transactions, receipts, users, tickets

- **Bookings Database(Read only)** - Customer bookings, domestic/international sales data

## Cron Jobs & Automation

### Bank Data Sync Cron

- **Schedule**: Daily at midnight (Asia/Kolkata timezone)

- **Purpose**: Automated refund sync

### Available Scripts:

```bash
	# Manual Operations
	yarn  bank:sync  # Manual bank data sync
	yarn  bank:refund:sync  # Manual refund data sync
	yarn  update:payments  # Update payment collections
	yarn  fetch:sheet  # Fetch spreadsheet data

	# Migration Scripts
	yarn  migrate:sales-receipts  # Migrate sales receipts from sheet
	yarn  migrate:invoices  # Migrate invoice receipts
	yarn  migrate:marked-transactions  # Migrate marked transactions
	yarn  migrate:sales-receipts-dump  # Migrate from data dump
```

### Automated Processes:

1.  **Daily Bank Data Import** - Processes new transactions

2.  **Receipt Generation Queue** - Continuous background processing

3.  **Email Delivery System** - Automated customer communications

4.  **Payment Reminder Scheduling** - EasyCron integration for timely reminders

## API Endpoints

### Authentication Endpoints

| POST | `/api/v1/auth/login` | User login |

| POST | `/api/v1/auth/logout` | User logout |

| POST | `/api/v1/auth/forgot-password` | Forgot password |

| PUT | `/api/v1/auth/update-password` | Update password |

### User Management

| GET | `/api/v1/users` | Get all users |

### Bank Transactions

| GET | `/api/v1/bank-transactions` | Get all transactions |

| POST | `/api/v1/bank-transactions/bulk` | Create bulk transactions |

| PUT | `/api/v1/bank-transactions/:id/mark` | Mark transaction with booking |

| GET | `/api/v1/bank-transactions/unmarked` | Get unmarked transactions |

| POST | `/api/v1/bank-transactions/populate-s3` | Populate from S3 |

### Payment Collections

| GET | `/api/v1/payment-collection` | Get payment collections |

| POST | `/api/v1/payment-collection` | Create payment collection |

| PUT | `/api/v1/payment-collection/:id` | Update payment collection |

| GET | `/api/v1/payment-collection/due-payments` | Get due payments |

### Sales Receipts

| GET | `/api/v1/sales-receipts` | Get all receipts |

| POST | `/api/v1/sales-receipts/generate` | Generate new receipt |

| POST | `/api/v1/sales-receipts/send-mail` | Send receipt via email |

| PUT | `/api/v1/sales-receipts/:id/retry` | Retry failed receipt |

### Credit Notes

| GET | `/api/v1/credit-notes` | Get all credit notes |

| POST | `/api/v1/credit-notes` | Create credit note |

| PUT | `/api/v1/credit-notes/:id` | Update credit note |

| POST | `/api/v1/credit-notes/:id/redeem` | Redeem credit note |

### Reminders

| GET | `/api/v1/reminders` | Get all reminders |

| POST | `/api/v1/reminders` | Create reminder |

| PUT | `/api/v1/reminders/:id` | Update reminder |

| POST | `/api/v1/reminders/bulk` | Create bulk reminders |

### Tickets

| GET | `/api/v1/tickets` | Get all tickets |

| POST | `/api/v1/tickets` | Create ticket |

| GET | `/api/v1/tickets/:id` | Get ticket by ID |

| PUT | `/api/v1/tickets/:id` | Update ticket |

| POST | `/api/v1/tickets/:id/comments` | Add comment to ticket |

### Queue Management

| GET | `/api/v1/receipt-queue/status` | Get queue status |

| POST | `/api/v1/receipt-queue/retry-failed` | Retry failed jobs |

| POST | `/api/v1/receipt-queue/restart` | Restart queue processor |

### Bookings

| GET | `/api/v1/bookings` | Get all bookings |

| GET | `/api/v1/bookings/:id` | Get booking by ID |

| PUT | `/api/v1/bookings/:id/payment` | Update booking payment |

| GET | `/api/v1/bookings/:id/transactions` | Get booking transactions |

### API Client Management

| GET | `/api/v1/apiclient` | Get all API clients |

| POST | `/api/v1/apiclient` | Create API client |

| PUT | `/api/v1/apiclient/:id` | Update API client |

| DELETE | `/api/v1/apiclient/:id` | Delete API client |

| POST | `/api/v1/apiclient/:id/refresh-token` | Refresh API token |

### Export Services

| POST | `/api/v1/export-sales` | Export sales receipts |

| GET | `/api/v1/export-sales/download` | Download sales export |

| POST | `/api/v1/export-invoice` | Export invoice receipts |

| GET | `/api/v1/export-invoice/download` | Download invoice export |

### Invoice Management

| GET | `/api/v1/invoice-receipts` | Get all invoice receipts |

| POST | `/api/v1/invoice-receipts/generate` | Generate invoice receipt |

| GET | `/api/v1/invoice-receipts/:id` | Get invoice receipt by ID |

| PUT | `/api/v1/invoice-receipts/:id/retry` | Retry failed invoice |

### Invoice Reminders

| GET | `/api/v1/invoice-reminders` | Get all invoice reminders |

| POST | `/api/v1/invoice-reminders` | Create invoice reminder |

| PUT | `/api/v1/invoice-reminders/:id` | Update invoice reminder |

| DELETE | `/api/v1/invoice-reminders/:id` | Delete invoice reminder |

### CCR Data Management

| GET | `/api/v1/ccr-data` | Get CCR data |

| POST | `/api/v1/ccr-data` | Create CCR data |

| PUT | `/api/v1/ccr-data/:id` | Update CCR data |

### Advanced Queue Management

| GET | `/api/v1/receipt-queue/jobs` | Get queue jobs |

| POST | `/api/v1/receipt-queue/clear` | Clear queue |

| GET | `/api/v1/pdf-queue/status` | Get PDF queue status |

| POST | `/api/v1/pdf-queue/retry-failed` | Retry failed PDF jobs |

### Bank Details Management

| GET | `/api/v1/bank-details` | Get all bank details |

| POST | `/api/v1/bank-details` | Create bank details |

| PUT | `/api/v1/bank-details/:id` | Update bank details |

| DELETE | `/api/v1/bank-details/:id` | Delete bank details |

## API Client Security

### Overview

The FMS system implements a robust API client security mechanism that allows external systems to securely access the API using API keys and tokens.

### Security Architecture

The API client security system implements a **multi-layered security approach**:

#### 1. **Client Layer (External Systems)**

- External applications and services

- Hold API keys and client credentials

- Make authenticated requests to the API

#### 2. **Gateway Layer (API Middleware)**

- **Authentication Middleware**: Validates API keys and client emails

- **Rate Limiting**: Prevents abuse with configurable limits

- **Request Filtering**: Sanitizes and validates incoming requests

#### 3. **Backend Layer (FMS Services)**

- **Business Logic**: Processes authenticated requests

- **Data Validation**: Ensures data integrity

#### 4. **Security Components**

- **Redis Cache**: Stores session tokens and cron data

- **Encryption**: Secure data transmission and storage

### API Client Flow

1.  **Client Registration**

- External system registers as API client

- System generates unique API key and email

- Client receives credentials via secure channel

2.  **Authentication Process**

```bash
	# Client makes request with headers
	x-api-key:  your-api-key
	x-api-email:  client@example.com
```

3.  **Token Validation**

- Middleware validates API key and email combination

- Checks token expiration and permissions

4.  **Request Processing**

- Validated requests proceed to business logic

- Rate limiting prevents abuse

### Security Features

1.  **API Key Authentication**

- Unique API keys for each client

- Email-based client identification

- Secure key generation and storage

2.  **Request Validation**

- Input sanitization

- Schema validation

### Usage Examples

```bash
# Authenticate with API client
curl  -X  POST  https://api.wanderon.in/api/v1/auth/api-login  \
-H  "x-api-key: sk_live_abc123..."  \
-H  "x-api-email: client@example.com"
```

## Database Schema

### Core Entities

#### User Schema

```typescript
{
	email: String (unique, required),			//user email
	role: String (enum: EMPLOYEE_TYPES),		//user role
	name: String (required),					//user name
	phone: String (required),					//user phone
	password: String (required, select: false)	//password
}
```

#### BankTransaction Schema

```typescript
{
	uniqueId: String (unique, required),				//source_date_referenceId
	referenceId: String (required),						//transaction reference id
	narration: String (optional),						//transaction narration
	date: Date (required),								//transaction date
	creditAmount: Number (default: 0),					//credit amount
	debitAmount: Number (default: 0),					//debit amount
	source: String (enum: SOURCE_BANK_TYPE),			//bank source
	closingBalance: Number (default: 0),				//closing balance
	bookingIdsMap: [BookingTransactionItemSchema],		//booking ids mapped against txn amount,settelmentAmount, finalAmount
	isMarked: Boolean (default: false),					//flag txn marked or not
	fee: Number (default: 0),							//fee including tax (razorpay)
	tax: Number (default: 0),							//tax only (razorpay)
	gatewayCharges: Number (default: 0),				//gatewaycharges (razorpay)
	type: String (optional),							//b2b or b2c
	createdBy: String (required),						//who created
	updatedBy: String (required),						//who updated
	remarks: String (optional)							//remarks
}
```

#### Sales Receipt Schema (BookingReceiptHistory)

```typescript
{
	seriesNumber: String (required, unique),			//unique series number for receipt
	source: String (required),							//bank name or transaction source
	bookingId: String (required),						//booking id for which receipt is generated
	transactionId: ObjectId (required),					//transaction mongo object id
	uniqueId: String (required),						//transaction unique identifier
	amount: Number (required),							//transaction amount
	finalAmount: Number (required),						//final calculated amount after adjustments
	settlementAmount: Number (required),				//amount settled to merchant account
	baseAmount: Number (required),						//base amount before taxes
	totalAmountReceived: Number (default: 0),			//total amount received for this booking
	state: String,										//customer state for GST calculation
	gstNumber: String,									//customer GST number for B2B transactions
	companyName: String,								//company name for B2B transactions
	iGST: Number (default: 0),							//integrated goods and services tax
	cGST: Number (default: 0),							//central goods and services tax
	sGST: Number (default: 0),							//state goods and services tax
	tcs: Number (default: 0),							//tax collected at source
	isEmailSent: Boolean (default: false),				//flag for email delivery status
	isSalesReceiptGenerated: Boolean (default: false),	//flag for receipt generation status
	salesReceiptUrl: String,							//S3 URL of generated receipt PDF
	status: String (enum: ['PENDING', 'SUCCESS', 'FAILED']),	//receipt generation status
	isValid: Boolean (default: true),					//flag for receipt validity
	receiptType: String (enum: ['B2B', 'B2C'], default: 'B2C'),	//business type classification
	remarks: String,									//additional notes or comments
	receiptVersions: Array (default: [])					//version history of receipt updates
}
```

#### PaymentCollection Schema

```typescript
{
	tripId: String (required, unique),				//unique trip identifier
	tripName: String (required),					//name of the trip/package
	category: String (required),					//trip category (domestic/international)
	departureDate: Date (required),					//trip departure date
	bookingsCount: Number (required),				//total number of bookings for this trip
	revenue: Number (required),						//total revenue expected from trip
	totalPaymentReceived: Number (required),		//total payments received so far
	totalRefund: Number (required, default: 0),		//total refund amount processed
	activeBookings: [String] (required),			//list of active booking IDs
	createdBy: String (default: 'system'),			//who created this record
	updatedBy: String (default: 'system')			//who last updated this record
}
```

#### Credit Notes Schema

```typescript
{
	creditNoteId: String (required, unique),			//unique credit note identifier
	issuedOnDate: Date (required),					//date when credit note was issued
	oldBookingId: String (required),					//original booking ID for which credit note was issued
	amount: Number (required),						//credit note amount
	type: String (required),							//type of credit note (refund/adjustment)
	subType: String (required),						//sub-type classification
	validityDate: Date (required),					//expiry date of credit note
	bookingIds: [BookingDetails] (required),			//bookings where this credit note can be used
	bookingIdSearchMap: String (default: ''),			//searchable string of booking IDs
	redeemStatus: String (default: 'UNCLAIMED'),		//redemption status of credit note
	isExpired: Boolean (default: false)				//flag indicating if credit note has expired
}
```

#### Ticket Schema

```typescript
{
	transactionId: ObjectId (required, ref: 'BankTransaction'),		// the ticket raised for which transaction
	ticketId: String (required, unique), 						 	// ticket number
	referenceId: String (required),									// txn reference id
	uniqueId: String (required),									// txn unique identifier
	originalyMapped: {												// data marked originally
	gatewayCharges: Number,
	settlementAmount: Number,
	bookingIdMap: [BookingTransactionItem]
	},
	proposedChanges: {												// corrected data change
	gatewayCharges: Number,
	settlementAmount: Number,
	bookingIdMap: [BookingTransactionItem]
	},
	issue: String (enum: ['Transaction'], default: 'Transaction'),
	status: String (enum: ['unresolved', 'resolved', 'rejected']),
	remarks: String,
	proofs: [String],												// proof url for the correction
	raisedBy: String (required),
	assignee: [String] (required),									// to whom ticket is assigned
	assigneeActionTaken: Boolean (default: false),
	ticketThread: [ObjectId] (ref: 'TicketThread'),					// ticket actions
	isActive: Boolean (default: true)
}
```

#### Reminder Schema

```typescript
{
	bookingId: String (required),								//booking id
	bankDetailsId: ObjectId (ref: 'BankDetails', required),		//company bank details
	clientData: {
		email: String,
		name: String (required),
		phone: String (required),
		additionalInfo: Mixed
	},
	dueAmount: Number (required),								//Amount left to be paid
	scheduledAt: Date (required),								//reminder time
	deadLine: Date (required),									//payment deadline
	status: String (enum: ['pending', 'sent', 'failed', 'deactivated']),
	easyCronJobId: String,										//cron id
	isActive: Boolean (default: true),
	retryCount: Number (default: 0),
	maxRetries: Number (default: 3),
	agentData: {
		name: String,											//who is sending
		phone: String											//sender phone number
	}
}
```

#### API Client Schema

```typescript
{
	email: String (required, unique),			//email of api client
	name: String (required),					//name of client
	isEmailVerified: Boolean (default: false),	//is client email verified?
	role: String (required),					//role
	token: String (required, select: false),	//unique token
	isActive: Boolean (default: true)			//is client active
}

```

#### Receipt Queue Schema

```typescript
{
	status: String (enum: ['PENDING', 'SUCCESS', 'FAILED'], default: 'PENDING'), //status of job
	jobData: Object (required), 					//job payload
	isInProgress: Boolean (default: false),			//job processing
	processingStartedAt: Date						//job processing data&time
}
```

#### Invoice Receipt Schema

```typescript
{
	entryDate: Date, 					//invoice creation date
	series: String,  					//unique series for invoice FY202021/B2C03
	bookingId: String, 					//against invoice generated
	status: String, 					//pending, success, failed
	salesReceiptNo: String, 			//sales receipts number
	destination: String (default: ''), 	//trip destination
	sellingAmountPerPax: Number, 		//unit cost
	noOfPax: Number, 					//no of travelers
	totalSellingAmount: Number, 		//total cost
	totalAmountReceived: Number, 		//total payment received
	baseAmount: Number, 				//base amount
	igst: Number, 						//integrated Goods and Services Tax
	cgst: Number, 						//central gst
	sgst: Number, 						//state gst
	tcs: Number,  						//tcs
	invoiceLink: String (default: ''),	//s3 uploaded link
	emailSent: Boolean (default: false),//Flag for is email sent or not
	receiptType: String (default: 'B2C'),//B2B or B2C
	isValid: Boolean (default: true),	//for validity(be always true)
	b2bName: String (default: ''),		//for b2b case
	b2bGstNo: String (default: '')		//for b2b case
}



```

## Email Template System

### Template Architecture

Comprehensive email template system with HTML and text versions:

#### Available Templates:

- **Sales Receipt** - Payment confirmation emails

- **Invoice** - Invoice generation notifications

- **Sales Receipt Apology** - Payment update corrections

- **Wrong Marking Apology** - Transaction correction notifications

- **Credit Note Redemption** - Credit note usage confirmations

- **Ticket Assignment** - Staff notification for ticket assignments

- **Account Management** - User verification, password reset, welcome emails

- **API Client Token** - Partner authentication credentials

#### Template Features:

- **Zod Validation** - Type-safe template data validation

- **Nunjucks Rendering** - Dynamic content generation

- **Multi-format Support** - HTML and plain text versions

- **Custom Subjects** - Dynamic subject line generation

- **Attachment Support** - PDF receipts, invoices, and documents

#### Template Usage Example:

```typescript
// Emit email event
eventEmitter.emit(EventStore.SEND_MAIL, 'salesReceipt', customerEmail, {
  clientName: 'John Doe',
  bookingId: 'WO123456',
  date: '2024-01-15',
  receiptUrl: 'https://s3.bucket/receipt.pdf',
});
```

#### Email Configuration:

- **Service**: Gmail SMTP

- **Security**: App-specific passwords

- **Delivery Tracking**: Email sent status in database

- **Template Storage**: File-based with automatic compilation

## Support Document

- **User Manual**: [fms end user manual](https://docs.google.com/document/d/141sh5Ca35eJvW42oneFnEJYT6xm6UmxDpWOQibs6ZJ0/edit?usp=sharing)

---
