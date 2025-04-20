# Integration Adapters Specifications

## Task: T1-A-009
**Status**: Pending  
**Priority**: High  
**Dependencies**: None  
**Effort**: 2 points

## Description
Detailed documentation of the Integration Adapters specifications, including protocol-specific implementations, connection pool configurations, and retry policy specifications.

## Protocol Implementations

### MRHB Adapter

#### REST API Implementation
```typescript
interface MRHBAdapter {
  // Connection Configuration
  baseUrl: string;
  apiVersion: string;
  timeout: number;
  
  // Authentication
  authenticate(): Promise<string>;
  refreshToken(): Promise<string>;
  
  // Core Operations
  createTransaction(data: TransactionData): Promise<TransactionResponse>;
  getTransaction(id: string): Promise<TransactionResponse>;
  updateAccount(accountId: string, data: AccountData): Promise<AccountResponse>;
}

// Configuration
const mrhbConfig = {
  baseUrl: "https://api.mrhb.com",
  apiVersion: "v1",
  timeout: 30000,
  maxRetries: 3,
  retryDelay: 1000
};
```

#### WebSocket Implementation
```typescript
interface MRHBWebSocket {
  // Connection Management
  connect(): Promise<void>;
  disconnect(): void;
  reconnect(): Promise<void>;
  
  // Event Handling
  subscribe(channel: string): void;
  unsubscribe(channel: string): void;
  onMessage(handler: (data: any) => void): void;
  
  // Error Handling
  onError(handler: (error: Error) => void): void;
  onClose(handler: () => void): void;
}
```

### Creditcoin Adapter

#### Blockchain Protocol Implementation
```typescript
interface CreditcoinAdapter {
  // Network Configuration
  network: "mainnet" | "testnet";
  nodeUrl: string;
  chainId: number;
  
  // Wallet Management
  createWallet(): Promise<Wallet>;
  importWallet(privateKey: string): Promise<Wallet>;
  
  // Transaction Operations
  sendTransaction(tx: Transaction): Promise<TransactionReceipt>;
  getTransactionStatus(hash: string): Promise<TransactionStatus>;
  
  // Smart Contract Interactions
  callContract(method: string, params: any[]): Promise<any>;
  estimateGas(tx: Transaction): Promise<number>;
}

// Configuration
const creditcoinConfig = {
  network: "mainnet",
  nodeUrl: "https://rpc.creditcoin.org",
  chainId: 1,
  gasLimit: 21000,
  maxPriorityFee: 2
};
```

## Connection Pool Configurations

### MRHB Connection Pool
```yaml
pool:
  maxSize: 50
  minSize: 5
  idleTimeout: 300000
  maxLifetime: 1800000
  validationQuery: "SELECT 1"
  validationInterval: 30000
  connectionTimeout: 10000
  acquireTimeout: 10000
  leakDetectionThreshold: 300000
```

### Creditcoin Connection Pool
```yaml
pool:
  maxSize: 100
  minSize: 10
  idleTimeout: 600000
  maxLifetime: 3600000
  validationQuery: "eth_blockNumber"
  validationInterval: 60000
  connectionTimeout: 30000
  acquireTimeout: 30000
  leakDetectionThreshold: 600000
```

## Retry Policies

### MRHB Retry Policy
```yaml
retry:
  maxAttempts: 3
  initialInterval: 1000
  multiplier: 2.0
  maxInterval: 10000
  randomFactor: 0.2
  retryableExceptions:
    - "NetworkException"
    - "TimeoutException"
    - "RateLimitException"
  nonRetryableExceptions:
    - "AuthenticationException"
    - "ValidationException"
```

### Creditcoin Retry Policy
```yaml
retry:
  maxAttempts: 5
  initialInterval: 2000
  multiplier: 1.5
  maxInterval: 30000
  randomFactor: 0.3
  retryableExceptions:
    - "NonceTooLowError"
    - "InsufficientFundsError"
    - "NetworkError"
  nonRetryableExceptions:
    - "InvalidSignatureError"
    - "InvalidAddressError"
```

## Error Handling

### MRHB Error Handling
```typescript
class MRHBError extends Error {
  constructor(
    public code: string,
    public message: string,
    public statusCode: number,
    public retryable: boolean
  ) {
    super(message);
  }
}

// Error Codes
const MRHBErrorCodes = {
  NETWORK_ERROR: "NETWORK_ERROR",
  TIMEOUT: "TIMEOUT",
  RATE_LIMIT: "RATE_LIMIT",
  AUTHENTICATION: "AUTHENTICATION",
  VALIDATION: "VALIDATION"
};
```

### Creditcoin Error Handling
```typescript
class CreditcoinError extends Error {
  constructor(
    public code: string,
    public message: string,
    public transactionHash?: string,
    public retryable: boolean
  ) {
    super(message);
  }
}

// Error Codes
const CreditcoinErrorCodes = {
  NETWORK_ERROR: "NETWORK_ERROR",
  INSUFFICIENT_FUNDS: "INSUFFICIENT_FUNDS",
  NONCE_TOO_LOW: "NONCE_TOO_LOW",
  INVALID_SIGNATURE: "INVALID_SIGNATURE",
  INVALID_ADDRESS: "INVALID_ADDRESS"
};
```

## Monitoring and Metrics

### Connection Pool Metrics
- Active connections
- Idle connections
- Waiting threads
- Connection creation time
- Connection acquisition time
- Connection validation failures

### Retry Metrics
- Retry attempts
- Retry success rate
- Average retry delay
- Retry failure reasons
- Circuit breaker state

## Revision History

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 2024-04-19 | 1.0 | Initial draft | Technical Team | 