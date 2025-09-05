# API Endpoints

## Authentication Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/auth/login` | User login |
| POST | `/api/v1/auth/logout` | User logout |
| POST | `/api/v1/auth/forgot-password` | Forgot password |
| PUT | `/api/v1/auth/update-password` | Update password |

## User Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/users` | Get all users |

## Bank Transactions

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/bank-transactions` | Get all transactions |
| POST | `/api/v1/bank-transactions/bulk` | Create bulk transactions |
| PUT | `/api/v1/bank-transactions/:id/mark` | Mark transaction with booking |
| GET | `/api/v1/bank-transactions/unmarked` | Get unmarked transactions |
| POST | `/api/v1/bank-transactions/populate-s3` | Populate from S3 |

## Payment Collections

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/payment-collection` | Get payment collections |
| POST | `/api/v1/payment-collection` | Create payment collection |
| PUT | `/api/v1/payment-collection/:id` | Update payment collection |
| GET | `/api/v1/payment-collection/due-payments` | Get due payments |

## Sales Receipts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/sales-receipts` | Get all receipts |
| POST | `/api/v1/sales-receipts/generate` | Generate new receipt |
| POST | `/api/v1/sales-receipts/send-mail` | Send receipt via email |
| PUT | `/api/v1/sales-receipts/:id/retry` | Retry failed receipt |

## Credit Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/credit-notes` | Get all credit notes |
| POST | `/api/v1/credit-notes` | Create credit note |
| PUT | `/api/v1/credit-notes/:id` | Update credit note |
| POST | `/api/v1/credit-notes/:id/redeem` | Redeem credit note |

## Reminders

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/reminders` | Get all reminders |
| POST | `/api/v1/reminders` | Create reminder |
| PUT | `/api/v1/reminders/:id` | Update reminder |
| POST | `/api/v1/reminders/bulk` | Create bulk reminders |

## Tickets

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/tickets` | Get all tickets |
| POST | `/api/v1/tickets` | Create ticket |
| GET | `/api/v1/tickets/:id` | Get ticket by ID |
| PUT | `/api/v1/tickets/:id` | Update ticket |
| POST | `/api/v1/tickets/:id/comments` | Add comment to ticket |

## Queue Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/receipt-queue/status` | Get queue status |
| POST | `/api/v1/receipt-queue/retry-failed` | Retry failed jobs |
| POST | `/api/v1/receipt-queue/restart` | Restart queue processor |

## Bookings

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/bookings` | Get all bookings |
| GET | `/api/v1/bookings/:id` | Get booking by ID |
| PUT | `/api/v1/bookings/:id/payment` | Update booking payment |
| GET | `/api/v1/bookings/:id/transactions` | Get booking transactions |

## API Client Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/apiclient` | Get all API clients |
| POST | `/api/v1/apiclient` | Create API client |
| PUT | `/api/v1/apiclient/:id` | Update API client |
| DELETE | `/api/v1/apiclient/:id` | Delete API client |
| POST | `/api/v1/apiclient/:id/refresh-token` | Refresh API token |

## Export Services

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/export-sales` | Export sales receipts |
| GET | `/api/v1/export-sales/download` | Download sales export |
| POST | `/api/v1/export-invoice` | Export invoice receipts |
| GET | `/api/v1/export-invoice/download` | Download invoice export |

## Invoice Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/invoice-receipts` | Get all invoice receipts |
| POST | `/api/v1/invoice-receipts/generate` | Generate invoice receipt |
| GET | `/api/v1/invoice-receipts/:id` | Get invoice receipt by ID |
| PUT | `/api/v1/invoice-receipts/:id/retry` | Retry failed invoice |

## Invoice Reminders

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/invoice-reminders` | Get all invoice reminders |
| POST | `/api/v1/invoice-reminders` | Create invoice reminder |
| PUT | `/api/v1/invoice-reminders/:id` | Update invoice reminder |
| DELETE | `/api/v1/invoice-reminders/:id` | Delete invoice reminder |

## CCR Data Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/ccr-data` | Get CCR data |
| POST | `/api/v1/ccr-data` | Create CCR data |
| PUT | `/api/v1/ccr-data/:id` | Update CCR data |

## Advanced Queue Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/receipt-queue/jobs` | Get queue jobs |
| POST | `/api/v1/receipt-queue/clear` | Clear queue |
| GET | `/api/v1/pdf-queue/status` | Get PDF queue status |
| POST | `/api/v1/pdf-queue/retry-failed` | Retry failed PDF jobs |

## Bank Details Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v1/bank-details` | Get all bank details |
| POST | `/api/v1/bank-details` | Create bank details |
| PUT | `/api/v1/bank-details/:id` | Update bank details |
| DELETE | `/api/v1/bank-details/:id` | Delete bank details |