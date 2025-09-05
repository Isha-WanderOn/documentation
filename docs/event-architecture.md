# Event-Driven Architecture

## Custom Event Emitter

The system uses a custom event emitter for loose coupling between services:

### Key Events:

- `SEND_MAIL` - Email sending events
- `API_CLIENT_*` - API client management events
- `BOOKING_MARKED_TO_TRANSACTION` - Transaction marking events
- `NEW_RECEIPT_JOB` - Receipt generation events
- `CREDIT_NOTES_MIGRATION` - Migration events
- `UPDATE_PAYMENT_COLLECTIONS_SCRIPT` - Payment update events

### Event Flow Example:

```typescript
// Emit event
eventEmitter.emit(EventStore.SEND_MAIL, templateName, to, data);
// Listen for event
eventEmitter.on(EventStore.SEND_MAIL, async (template, email, data) => {
  await mailer.createMailAndSend(template, email, data);
});
```