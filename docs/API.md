# Maven Plus Ltd - API Documentation

## Base URL

```
Development: http://localhost:5000/api
Production: https://api.mavenplus.ltd/api
```

## API Versioning

Current Version: `v1`

All endpoints are prefixed with `/v1`

## Authentication

### Headers

All authenticated endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

### Response Format

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {},
  "errors": null
}
```

---

## Authentication Endpoints

### Register User

**POST** `/v1/auth/register`

Request:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+2348012345678",
  "password": "SecurePass123!",
  "role": "sales_rep"
}
```

Response:
```json
{
  "success": true,
  "message": "Registration successful. Please verify your email.",
  "data": {
    "userId": "usr_12345",
    "email": "john@example.com",
    "role": "sales_rep",
    "verificationRequired": true
  }
}
```

### Login

**POST** `/v1/auth/login`

Request:
```json
{
  "email": "john@example.com",
  "password": "SecurePass123!",
  "rememberMe": true
}
```

Response:
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "usr_12345",
      "email": "john@example.com",
      "firstName": "John",
      "lastName": "Doe",
      "role": "sales_rep"
    },
    "requiresOTP": true
  }
}
```

### Verify OTP

**POST** `/v1/auth/verify-otp`

Request:
```json
{
  "email": "john@example.com",
  "otp": "123456"
}
```

Response:
```json
{
  "success": true,
  "message": "OTP verified successfully",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "usr_12345",
      "email": "john@example.com",
      "role": "sales_rep"
    }
  }
}
```

### Forgot Password

**POST** `/v1/auth/forgot-password`

Request:
```json
{
  "email": "john@example.com"
}
```

Response:
```json
{
  "success": true,
  "message": "Password reset link sent to email"
}
```

### Reset Password

**POST** `/v1/auth/reset-password`

Request:
```json
{
  "token": "reset_token_from_email",
  "newPassword": "NewSecurePass456!"
}
```

Response:
```json
{
  "success": true,
  "message": "Password reset successfully"
}
```

### Logout

**POST** `/v1/auth/logout`

Response:
```json
{
  "success": true,
  "message": "Logout successful"
}
```

---

## Product Endpoints

### Get All Products

**GET** `/v1/products`

Query Parameters:
- `page`: 1 (default)
- `limit`: 20 (default)
- `category`: Filter by category
- `search`: Search term
- `status`: active, inactive, discontinued

Response:
```json
{
  "success": true,
  "data": {
    "products": [
      {
        "id": "prod_12345",
        "name": "Laptop Pro",
        "category": "Electronics",
        "price": 500000,
        "currency": "₦",
        "quantity": 15,
        "status": "active",
        "image": "https://cdn.mavenplus.ltd/image.jpg",
        "description": "High-performance laptop"
      }
    ],
    "pagination": {
      "total": 150,
      "page": 1,
      "limit": 20,
      "pages": 8
    }
  }
}
```

### Get Product By ID

**GET** `/v1/products/:productId`

Response:
```json
{
  "success": true,
  "data": {
    "id": "prod_12345",
    "name": "Laptop Pro",
    "category": "Electronics",
    "price": 500000,
    "quantity": 15,
    "reorderLevel": 5,
    "status": "active",
    "description": "High-performance laptop",
    "image": "https://cdn.mavenplus.ltd/image.jpg",
    "createdAt": "2026-01-15T10:30:00Z",
    "updatedAt": "2026-05-08T14:22:00Z"
  }
}
```

### Create Product

**POST** `/v1/products` (Admin/Manager only)

Request:
```json
{
  "name": "Desktop Computer",
  "category": "Electronics",
  "price": 800000,
  "quantity": 10,
  "reorderLevel": 5,
  "description": "Powerful desktop workstation",
  "image": "file_upload_or_url"
}
```

Response:
```json
{
  "success": true,
  "message": "Product created successfully",
  "data": {
    "id": "prod_67890",
    "name": "Desktop Computer",
    "category": "Electronics",
    "price": 800000,
    "quantity": 10
  }
}
```

### Update Product

**PUT** `/v1/products/:productId` (Admin/Manager only)

Request:
```json
{
  "name": "Desktop Computer Pro",
  "price": 850000,
  "quantity": 12
}
```

Response:
```json
{
  "success": true,
  "message": "Product updated successfully",
  "data": {
    "id": "prod_67890",
    "name": "Desktop Computer Pro",
    "price": 850000,
    "quantity": 12
  }
}
```

### Delete Product

**DELETE** `/v1/products/:productId` (Admin only)

Response:
```json
{
  "success": true,
  "message": "Product deleted successfully"
}
```

---

## Sales Endpoints

### Create Sale/Order

**POST** `/v1/sales`

Request:
```json
{
  "customerId": "cust_12345",
  "items": [
    {
      "productId": "prod_12345",
      "quantity": 2,
      "unitPrice": 500000
    },
    {
      "productId": "prod_67890",
      "quantity": 1,
      "unitPrice": 800000
    }
  ],
  "discount": 50000,
  "taxRate": 0.075,
  "paymentMethod": "card",
  "paymentGateway": "paystack"
}
```

Response:
```json
{
  "success": true,
  "message": "Sale recorded successfully",
  "data": {
    "saleId": "sale_12345",
    "invoiceId": "inv_12345",
    "customerId": "cust_12345",
    "items": [
      {
        "productId": "prod_12345",
        "quantity": 2,
        "unitPrice": 500000,
        "subtotal": 1000000
      }
    ],
    "subtotal": 1800000,
    "discount": 50000,
    "tax": 131250,
    "total": 1881250,
    "paymentStatus": "completed",
    "timestamp": "2026-05-08T15:30:00Z"
  }
}
```

### Get Sales History

**GET** `/v1/sales`

Query Parameters:
- `page`: 1
- `limit`: 20
- `startDate`: YYYY-MM-DD
- `endDate`: YYYY-MM-DD
- `status`: completed, pending, cancelled

Response:
```json
{
  "success": true,
  "data": {
    "sales": [
      {
        "saleId": "sale_12345",
        "customerId": "cust_12345",
        "total": 1881250,
        "status": "completed",
        "timestamp": "2026-05-08T15:30:00Z"
      }
    ],
    "pagination": {
      "total": 450,
      "page": 1,
      "limit": 20,
      "pages": 23
    }
  }
}
```

### Generate Receipt

**GET** `/v1/sales/:saleId/receipt`

Response:
```json
{
  "success": true,
  "data": {
    "receiptUrl": "https://cdn.mavenplus.ltd/receipts/receipt_12345.pdf",
    "receiptHTML": "<!-- HTML receipt content -->"
  }
}
```

---

## Customer Endpoints

### Get All Customers

**GET** `/v1/customers`

Response:
```json
{
  "success": true,
  "data": {
    "customers": [
      {
        "id": "cust_12345",
        "name": "Acme Corporation",
        "email": "contact@acme.com",
        "phone": "+2348012345678",
        "address": "123 Business Street",
        "totalSpent": 5000000,
        "lastPurchase": "2026-05-08T10:00:00Z",
        "outstandingBalance": 500000
      }
    ]
  }
}
```

### Create Customer

**POST** `/v1/customers`

Request:
```json
{
  "name": "Tech Solutions Ltd",
  "email": "sales@techsolutions.com",
  "phone": "+2349876543210",
  "address": "456 Tech Park Avenue",
  "city": "Lagos",
  "state": "Lagos",
  "country": "Nigeria"
}
```

Response:
```json
{
  "success": true,
  "message": "Customer created successfully",
  "data": {
    "id": "cust_67890",
    "name": "Tech Solutions Ltd",
    "email": "sales@techsolutions.com"
  }
}
```

### Get Customer Details

**GET** `/v1/customers/:customerId`

Response:
```json
{
  "success": true,
  "data": {
    "id": "cust_12345",
    "name": "Acme Corporation",
    "email": "contact@acme.com",
    "phone": "+2348012345678",
    "address": "123 Business Street",
    "city": "Lagos",
    "state": "Lagos",
    "country": "Nigeria",
    "purchaseHistory": [
      {
        "saleId": "sale_12345",
        "total": 1881250,
        "date": "2026-05-08T15:30:00Z"
      }
    ],
    "totalSpent": 5000000,
    "outstandingBalance": 500000,
    "notes": "Premium customer"
  }
}
```

---

## Payment Endpoints

### Initiate Payment

**POST** `/v1/payments/initiate`

Request:
```json
{
  "amount": 1881250,
  "currency": "NGN",
  "saleId": "sale_12345",
  "gateway": "paystack",
  "customerEmail": "customer@example.com"
}
```

Response:
```json
{
  "success": true,
  "data": {
    "paymentId": "pay_12345",
    "authorizationUrl": "https://checkout.paystack.com/...",
    "accessCode": "wz2mco1sqa",
    "reference": "paystack_ref_12345"
  }
}
```

### Verify Payment

**POST** `/v1/payments/verify`

Request:
```json
{
  "reference": "paystack_ref_12345",
  "gateway": "paystack"
}
```

Response:
```json
{
  "success": true,
  "data": {
    "paymentId": "pay_12345",
    "status": "success",
    "amount": 1881250,
    "currency": "NGN",
    "timestamp": "2026-05-08T15:35:00Z"
  }
}
```

---

## Report Endpoints

### Daily Sales Report

**GET** `/v1/reports/daily`

Query Parameters:
- `date`: YYYY-MM-DD (default: today)

Response:
```json
{
  "success": true,
  "data": {
    "date": "2026-05-08",
    "totalSales": 5500000,
    "totalTransactions": 12,
    "totalCustomers": 8,
    "topProducts": [
      {
        "productId": "prod_12345",
        "name": "Laptop Pro",
        "unitsSold": 5,
        "revenue": 2500000
      }
    ]
  }
}
```

### Monthly Sales Report

**GET** `/v1/reports/monthly`

Query Parameters:
- `month`: MM (01-12)
- `year`: YYYY

Response:
```json
{
  "success": true,
  "data": {
    "month": "May",
    "year": 2026,
    "totalSales": 45000000,
    "totalTransactions": 156,
    "averageSaleValue": 288461.54,
    "topProducts": [],
    "topCustomers": [],
    "salesByDay": []
  }
}
```

### Export Report

**GET** `/v1/reports/export`

Query Parameters:
- `type`: daily, monthly, custom
- `format`: pdf, excel, csv
- `startDate`: YYYY-MM-DD
- `endDate`: YYYY-MM-DD

Response:
```json
{
  "success": true,
  "data": {
    "downloadUrl": "https://cdn.mavenplus.ltd/reports/report_12345.pdf",
    "fileName": "sales_report_2026_05.pdf"
  }
}
```

---

## Inventory Endpoints

### Get Inventory Status

**GET** `/v1/inventory`

Response:
```json
{
  "success": true,
  "data": {
    "totalProducts": 150,
    "lowStockItems": [
      {
        "productId": "prod_12345",
        "name": "Laptop Pro",
        "quantity": 3,
        "reorderLevel": 5,
        "status": "low"
      }
    ],
    "outOfStock": [
      {
        "productId": "prod_54321",
        "name": "External SSD",
        "quantity": 0
      }
    ]
  }
}
```

### Update Inventory

**PUT** `/v1/inventory/:productId`

Request:
```json
{
  "quantity": 25,
  "operation": "set"
}
```

Response:
```json
{
  "success": true,
  "message": "Inventory updated successfully",
  "data": {
    "productId": "prod_12345",
    "quantity": 25
  }
}
```

---

## Notification Endpoints

### Get Notifications

**GET** `/v1/notifications`

Response:
```json
{
  "success": true,
  "data": {
    "notifications": [
      {
        "id": "notif_12345",
        "type": "sale_completed",
        "title": "Sale Completed",
        "message": "Order INV-12345 completed successfully",
        "read": false,
        "timestamp": "2026-05-08T15:35:00Z"
      }
    ],
    "unreadCount": 5
  }
}
```

### Mark Notification as Read

**PUT** `/v1/notifications/:notificationId/read`

Response:
```json
{
  "success": true,
  "message": "Notification marked as read"
}
```

---

## Error Responses

### 400 Bad Request

```json
{
  "success": false,
  "message": "Invalid request parameters",
  "errors": {
    "email": "Invalid email format"
  }
}
```

### 401 Unauthorized

```json
{
  "success": false,
  "message": "Authentication required",
  "errors": null
}
```

### 403 Forbidden

```json
{
  "success": false,
  "message": "You do not have permission to perform this action",
  "errors": null
}
```

### 404 Not Found

```json
{
  "success": false,
  "message": "Resource not found",
  "errors": null
}
```

### 500 Internal Server Error

```json
{
  "success": false,
  "message": "Internal server error",
  "errors": null
}
```

---

## Rate Limiting

API rate limits:
- **Unauthenticated**: 60 requests/hour per IP
- **Authenticated**: 1000 requests/hour per user
- **Premium Users**: 10000 requests/hour

Rate limit headers:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1641234567
```

---

## Webhooks

### Payment Webhook

**POST** `https://your-domain.com/webhooks/payment`

```json
{
  "event": "payment.completed",
  "data": {
    "paymentId": "pay_12345",
    "amount": 1881250,
    "reference": "paystack_ref_12345",
    "timestamp": "2026-05-08T15:35:00Z"
  }
}
```

### Sale Webhook

**POST** `https://your-domain.com/webhooks/sale`

```json
{
  "event": "sale.created",
  "data": {
    "saleId": "sale_12345",
    "total": 1881250,
    "timestamp": "2026-05-08T15:30:00Z"
  }
}
```

---

**Version**: 1.0  
**Last Updated**: 2026-05-08
