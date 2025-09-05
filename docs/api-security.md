# API Client Security

## Overview

The FMS system implements a robust API client security mechanism that allows external systems to securely access the API using API keys and tokens.

## Security Architecture

The API client security system implements a **multi-layered security approach**:

### 1. Client Layer (External Systems)

- External applications and services
- Hold API keys and client credentials
- Make authenticated requests to the API

### 2. Gateway Layer (API Middleware)

- **Authentication Middleware**: Validates API keys and client emails
- **Rate Limiting**: Prevents abuse with configurable limits
- **Request Filtering**: Sanitizes and validates incoming requests

### 3. Backend Layer (FMS Services)

- **Business Logic**: Processes authenticated requests
- **Data Validation**: Ensures data integrity

### 4. Security Components

- **Redis Cache**: Stores session tokens and cron data
- **Encryption**: Secure data transmission and storage

## API Client Flow

1. **Client Registration**

- External system registers as API client
- System generates unique API key and email
- Client receives credentials via secure channel

2. **Authentication Process**

```bash
# Client makes request with headers
x-api-key: your-api-key
x-api-email: client@example.com
```

3. **Token Validation**

- Middleware validates API key and email combination
- Checks token expiration and permissions

4. **Request Processing**

- Validated requests proceed to business logic
- Rate limiting prevents abuse

## Security Features

1. **API Key Authentication**

- Unique API keys for each client
- Email-based client identification
- Secure key generation and storage

2. **Request Validation**

- Input sanitization
- Schema validation

## Usage Examples

```bash
# Authenticate with API client
curl -X POST https://api.wanderon.in/api/v1/auth/api-login \
-H "x-api-key: sk_live_abc123..." \
-H "x-api-email: client@example.com"
```