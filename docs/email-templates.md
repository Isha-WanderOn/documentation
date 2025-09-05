# Email Template System

## Template Architecture

Comprehensive email template system with HTML and text versions:

### Available Templates:

- **Sales Receipt** - Payment confirmation emails
- **Invoice** - Invoice generation notifications
- **Sales Receipt Apology** - Payment update corrections
- **Wrong Marking Apology** - Transaction correction notifications
- **Credit Note Redemption** - Credit note usage confirmations
- **Ticket Assignment** - Staff notification for ticket assignments
- **Account Management** - User verification, password reset, welcome emails
- **API Client Token** - Partner authentication credentials

### Template Features:

- **Zod Validation** - Type-safe template data validation
- **Nunjucks Rendering** - Dynamic content generation
- **Multi-format Support** - HTML and plain text versions
- **Custom Subjects** - Dynamic subject line generation
- **Attachment Support** - PDF receipts, invoices, and documents

### Template Usage Example:

```typescript
// Emit email event
eventEmitter.emit(EventStore.SEND_MAIL, 'salesReceipt', customerEmail, {
  clientName: 'John Doe',
  bookingId: 'WO123456',
  date: '2024-01-15',
  receiptUrl: 'https://s3.bucket/receipt.pdf',
});
```

### Email Configuration:

- **Service**: Gmail SMTP
- **Security**: App-specific passwords
- **Delivery Tracking**: Email sent status in database
- **Template Storage**: File-based with automatic compilation