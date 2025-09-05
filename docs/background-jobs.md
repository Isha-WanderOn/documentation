# Background Job Processing

## Queue Architecture

The FMS system uses **Mongo collection as an queue** for background processing:

### Queue Types:

- **Receipt Queue** - Processes sales receipt generation jobs
- **Invoice Queue** - Handles invoice generation tasks
- **PDF Queue** - Manages PDF creation via PDFCrowd API

### Smart Processor Features:

- Automatic restart capabilities
- Job retry mechanisms with configurable limits
- Queue monitoring and status tracking
- Parallel processing with controlled concurrency
- Failed job recovery and analysis

### Queue Flow Example:

```
Transaction Marked → Queue Job Created → PDF Generation → S3 Upload → Email Notification → Job Complete
```

### Queue Management:

- **Graceful shutdown** with job completion

## Queue Consumer Services:

- `ReceiptQueueConsumerService` - Smart processor for receipt generation
- `InvoiceReceiptQueueConsumerService` - Invoice processing automation
- `PdfQueueConsumerService` - PDF generation job handler