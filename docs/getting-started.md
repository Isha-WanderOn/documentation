# Getting Started

## Prerequisites

- Node.js 18.x or higher
- Yarn package manager
- MongoDB 5.x or higher
- Redis 6.x or higher

## Installation

```bash
# Clone the repository
git clone <repository-url>
cd fms-backend

# Install dependencies
yarn install

#create .env.development file

# Start development server
yarn run dev
```

## Environment Configuration

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