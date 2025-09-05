# Cron Jobs & Automation

## Bank Data Sync Cron

- **Schedule**: Daily at midnight (Asia/Kolkata timezone)
- **Purpose**: Automated refund sync

## Available Scripts:

```bash
# Manual Operations
yarn bank:sync # Manual bank data sync
yarn bank:refund:sync # Manual refund data sync
yarn update:payments # Update payment collections
yarn fetch:sheet # Fetch spreadsheet data

# Migration Scripts
yarn migrate:sales-receipts # Migrate sales receipts from sheet
yarn migrate:invoices # Migrate invoice receipts
yarn migrate:marked-transactions # Migrate marked transactions
yarn migrate:sales-receipts-dump # Migrate from data dump
```

## Automated Processes:

1. **Daily Bank Data Import** - Processes new transactions
2. **Receipt Generation Queue** - Continuous background processing
3. **Email Delivery System** - Automated customer communications
4. **Payment Reminder Scheduling** - EasyCron integration for timely reminders