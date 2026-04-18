# PayFlow API Documentation

**Author:** Chinemerem Chianu-Okoli  
*Technical writing sample: REST API documentation for a payment system*

---

## Overview

The PayFlow API allows developers to integrate secure and scalable payment workflows into applications.  
PayFlow supports authorization, refunds, payouts, and subscription billing using methods such as card, bank transfer, and wallet.

### Example workflows:
- Accept one-time payments using card, wallet, or bank transfer  
- Send payouts to vendors globally  
- Receive real-time payments  

PayFlow uses standard HTTPS status codes to indicate the result of a request.

---

## Base URL
https://api.payflow.com

---

## Endpoints

All endpoints accept and return JSON.

| Method | Endpoint                | Description                          |
|--------|------------------------|--------------------------------------|
| POST   | /payments              | Create a new payment                 |
| GET    | /payments              | List all payments                    |
| GET    | /payments/{id}         | Retrieve a payment by ID             |
| POST   | /payments/{id}/cancel  | Cancel a pending payment             |
| POST   | /payouts               | Create a payout to a bank account    |
| GET    | /payouts/{id}          | Retrieve a payout by ID              |
| POST   | /refund                | Issue a full refund                  |

---

## Authentication

PayFlow uses API keys to authenticate requests. You can manage your API keys from the PayFlow dashboard.  
API keys carry many privileges, so keep them secure and do not expose them publicly.
All API requests must be made over HTTPS. Requests without authentication will fail.

### Key Types

| Key Type | Prefix   | Usage                                      |
|----------|----------|--------------------------------------------|
| Test Key | pf_test_ | Development and testing only (no real charges) |
| Live Key | pf_live_ | Production use (real transactions)          |

### Authorization

This API uses bearer token authentication. Include your API key in the `Authorization` header:
Authorization: Bearer YOUR_API_KEY

### Headers

| Header        | Value                 | Description                     |
|---------------|----------------------|---------------------------------|
| Authorization | Bearer YOUR_API_KEY  | API key for authentication      |
| Content-Type  | application/json     | Request format                  |

### Example Authenticated Request
Headers:
Authorization: Bearer sk_test_abc123
Content-Type: application/json

---

## Payments

The Payments API allows you to create and manage payment transactions.

---

## Create a Payment

Initiates a new payment transaction for a customer.

### Request Parameters

| Parameter         | Type    | Required | Description                              |
|-------------------|--------|----------|------------------------------------------|
| amount            | integer | Yes      | Payment amount                           |
| currency          | string | Yes      | Currency code (e.g., NGN)                |
| description       | string | No       | Description shown on customer statement  |
| customer_id       | string | Yes      | Customer identifier                      |
| idempotency_key   | string | Yes      | Unique key to prevent duplicate requests |

---

### Sample Request
POST https://api.payflow.com/api/payments

Headers:
Authorization: Bearer pf_live_sk_abc123
Content-Type: application/json

{
"amount": 5000,
"currency": "NGN",
"customer_id": "cust_001",
"payment_method": "card"
}
### Sample Response

```json
{
  "status": "success",
  "transaction_id": "txn_12345",
  "amount": 5000,
  "currency": "NGN",
  "payment_status": "completed"
}

```
##  Idempotency

The API supports idempotency to safely retry requests without creating duplicate transactions.
When a request is sent with an Idempotency-Key, PayFlow stores the result of the first request associated with that key.
If the same key is used again, PayFlow returns the original response without creating a new payment.
This ensures duplicate requests do not result in multiple charges.

### Example Request

POST https://api.payflow.com/api/payments

**Headers:**
Authorization: Bearer sk_test_abc123  
Idempotency-Key: 123e4567-e89b-12d3-a456-426614174000  

```json
{
  "amount": 5000,
  "currency": "NGN",
  "customer_id": "cust_001",
  "payment_method": "card"
}

```

##  Error Handling

PayFlow uses HTTP status codes to indicate whether a request succeeded or failed.

2xx: Success,
4xx: Client errors,
5xx: Server errors

### HTTP Status Codes

| Code | Name            | Meaning |
|------|----------------|---------|
| 200  | OK             | Request succeeded |
| 201  | Created        | Resource created successfully |
| 400  | Bad Request    | Invalid parameters in request |
| 401  | Unauthorized   | Invalid API key |
| 402  | Request Failed | Valid request but failed |
| 403  | Forbidden      | Insufficient permissions |
| 404  | Not Found      | Resource not found |
| 409  | Conflict       | Duplicate idempotency key or conflict |
| 429  | Too Many Requests | Rate limit exceeded |
| 500  | Server Error   | Unexpected server error |

##  Notes
Send all requests over HTTPS
Include a valid API key in the Authorization header
Use a unique idempotency key for each payment request

##  Support
For support or questions, contact: support@payflow.com

