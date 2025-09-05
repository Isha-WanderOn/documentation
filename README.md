# WanderOn Finance Management System (FMS) Backend

A comprehensive finance management system designed for WanderOn travel company to handle financial transactions, payment collection, receipt generation, and customer communication automation.

## Table of Contents

- [Project Overview](#project-overview)
- [Technology Stack](docs/technology-stack.md)
- [System Architecture](docs/system-architecture.md)
- [Getting Started](docs/getting-started.md)
- [Background Job Processing](docs/background-jobs.md)
- [Event-Driven Architecture](docs/event-architecture.md)
- [Multi-Database Architecture](docs/database.md)
- [External Integrations](docs/external-integrations.md)
- [Cron Jobs & Automation](docs/cron-automation.md)
- [API Endpoints](docs/api-endpoints.md)
- [API Client Security](docs/api-security.md)
- [Email Template System](docs/email-templates.md)
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

## Support Document

- **User Manual**: [fms end user manual](https://docs.google.com/document/d/141sh5Ca35eJvW42oneFnEJYT6xm6UmxDpWOQibs6ZJ0/edit?usp=sharing)