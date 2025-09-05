# Multi-Database Architecture

## Database Separation:

- **FMS Database** - Financial data, transactions, receipts, users, tickets
- **Bookings Database(Read only)** - Customer bookings, domestic/international sales data

# Database Schema

## Core Entities

### User Schema

```typescript
{
	email: String (unique, required),			//user email
	role: String (enum: EMPLOYEE_TYPES),		//user role
	name: String (required),					//user name
	phone: String (required),					//user phone
	password: String (required, select: false)	//password
}
```

### BankTransaction Schema

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

### Sales Receipt Schema (BookingReceiptHistory)

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

### PaymentCollection Schema

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

### Credit Notes Schema

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

### Ticket Schema

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

### Reminder Schema

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

### API Client Schema

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

### Receipt Queue Schema

```typescript
{
	status: String (enum: ['PENDING', 'SUCCESS', 'FAILED'], default: 'PENDING'), //status of job
	jobData: Object (required), 					//job payload
	isInProgress: Boolean (default: false),			//job processing
	processingStartedAt: Date						//job processing data&time
}
```

### Invoice Receipt Schema

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