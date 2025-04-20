# API Gateway Specifications

## Task: T1-A-006
**Status**: Pending  
**Priority**: High  
**Dependencies**: None  
**Effort**: 2 points

## Description
Detailed documentation of the API Gateway specifications, including endpoint definitions, rate limiting rules, and WebSocket protocol implementation.

## API Endpoints

### Authentication Endpoints

#### POST /api/v1/auth/login
- **Purpose**: User authentication
- **Request Body**:
  ```json
  {
    "username": "string",
    "password": "string",
    "deviceId": "string"
  }
  ```
- **Response**:
  ```json
  {
    "token": "string",
    "refreshToken": "string",
    "expiresIn": "number",
    "userId": "string"
  }
  ```
- **Rate Limit**: 5 requests per minute per IP
- **Authentication**: None

#### POST /api/v1/auth/refresh
- **Purpose**: Token refresh
- **Request Body**:
  ```json
  {
    "refreshToken": "string"
  }
  ```
- **Response**: Same as login
- **Rate Limit**: 10 requests per minute per IP
- **Authentication**: None

### Transaction Endpoints

#### POST /api/v1/transactions
- **Purpose**: Create new transaction
- **Request Body**:
  ```json
  {
    "amount": "number",
    "currency": "string",
    "recipient": "string",
    "description": "string"
  }
  ```
- **Response**:
  ```json
  {
    "transactionId": "string",
    "status": "string",
    "timestamp": "string"
  }
  ```
- **Rate Limit**: 100 requests per minute per user
- **Authentication**: Required (Bearer token)

#### GET /api/v1/transactions/{id}
- **Purpose**: Get transaction details
- **Response**:
  ```json
  {
    "transactionId": "string",
    "amount": "number",
    "currency": "string",
    "status": "string",
    "timestamp": "string",
    "details": "object"
  }
  ```
- **Rate Limit**: 200 requests per minute per user
- **Authentication**: Required (Bearer token)

### Account Endpoints

#### GET /api/v1/accounts/balance
- **Purpose**: Get account balance
- **Response**:
  ```json
  {
    "balances": [
      {
        "currency": "string",
        "amount": "number",
        "available": "number"
      }
    ]
  }
  ```
- **Rate Limit**: 60 requests per minute per user
- **Authentication**: Required (Bearer token)

## Rate Limiting Rules

### Global Rules
- Maximum concurrent connections: 1000
- Maximum request size: 10MB
- Request timeout: 30 seconds

### Tiered Rate Limits
1. **Public Endpoints**
   - IP-based limits
   - 100 requests per minute per IP
   - Burst: 200 requests per minute

2. **Authenticated Endpoints**
   - User-based limits
   - 1000 requests per minute per user
   - Burst: 2000 requests per minute

3. **High-Priority Endpoints**
   - Custom limits based on business rules
   - Requires special authorization
   - Higher rate limits apply

### Rate Limit Headers
- `X-RateLimit-Limit`: Maximum requests per time window
- `X-RateLimit-Remaining`: Remaining requests in current window
- `X-RateLimit-Reset`: Time until rate limit resets (UTC timestamp)

## WebSocket Protocol

### Connection
- **URL**: `wss://api.mrhb-creditcoin.com/ws/v1`
- **Protocol**: WebSocket
- **Authentication**: Bearer token in query parameter

### Message Format
```json
{
  "type": "string",
  "payload": "object",
  "timestamp": "number"
}
```

### Event Types
1. **Transaction Events**
   - `transaction.created`
   - `transaction.updated`
   - `transaction.completed`
   - `transaction.failed`

2. **Account Events**
   - `balance.updated`
   - `account.updated`
   - `account.locked`

3. **System Events**
   - `system.maintenance`
   - `system.error`
   - `system.recovery`

### Error Handling
- **Connection Errors**: Automatic reconnection with exponential backoff
- **Message Errors**: Error response with code and description
- **Rate Limiting**: Connection closed with status code 429

### Heartbeat
- Interval: 30 seconds
- Timeout: 60 seconds
- Format: `{"type": "ping"}` / `{"type": "pong"}`

## Error Codes
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error
- 503: Service Unavailable

## Revision History

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 2024-04-19 | 1.0 | Initial draft | Technical Team | 