# Data Transformer Specifications

## Task: T1-A-001-a-1-3
**Status**: In Progress  
**Priority**: High  
**Dependencies**: None  
**Effort**: 1 point

## Description
Detailed documentation of the Data Transformer specifications, including schema definitions, transformation rules, and validation logic.

## Schema Definitions

### Transaction Schema

#### Source Schema (MRHB)
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["id", "amount", "currency", "status", "timestamp"],
  "properties": {
    "id": {
      "type": "string",
      "pattern": "^[A-Z0-9]{8}-[A-Z0-9]{4}-[A-Z0-9]{4}-[A-Z0-9]{4}-[A-Z0-9]{12}$"
    },
    "amount": {
      "type": "number",
      "minimum": 0.01,
      "maximum": 1000000
    },
    "currency": {
      "type": "string",
      "enum": ["USD", "EUR", "GBP", "BTC", "ETH"]
    },
    "status": {
      "type": "string",
      "enum": ["PENDING", "COMPLETED", "FAILED", "CANCELLED"]
    },
    "timestamp": {
      "type": "string",
      "format": "date-time"
    },
    "metadata": {
      "type": "object",
      "additionalProperties": true
    }
  }
}
```

#### Target Schema (Creditcoin)
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["transactionId", "value", "asset", "state", "createdAt"],
  "properties": {
    "transactionId": {
      "type": "string",
      "pattern": "^[a-f0-9]{64}$"
    },
    "value": {
      "type": "string",
      "pattern": "^[0-9]+(\\.[0-9]{1,8})?$"
    },
    "asset": {
      "type": "string",
      "enum": ["USD", "EUR", "GBP", "BTC", "ETH"]
    },
    "state": {
      "type": "string",
      "enum": ["PENDING", "CONFIRMED", "REJECTED", "CANCELLED"]
    },
    "createdAt": {
      "type": "integer",
      "minimum": 0
    },
    "attributes": {
      "type": "object",
      "additionalProperties": true
    }
  }
}
```

### Account Schema

#### Source Schema (MRHB)
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["accountId", "balances", "status"],
  "properties": {
    "accountId": {
      "type": "string",
      "pattern": "^ACC-[A-Z0-9]{8}$"
    },
    "balances": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["currency", "amount"],
        "properties": {
          "currency": {
            "type": "string",
            "enum": ["USD", "EUR", "GBP", "BTC", "ETH"]
          },
          "amount": {
            "type": "number",
            "minimum": 0
          }
        }
      }
    },
    "status": {
      "type": "string",
      "enum": ["ACTIVE", "SUSPENDED", "CLOSED"]
    }
  }
}
```

#### Target Schema (Creditcoin)
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["walletId", "holdings", "state"],
  "properties": {
    "walletId": {
      "type": "string",
      "pattern": "^[a-f0-9]{40}$"
    },
    "holdings": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["asset", "balance"],
        "properties": {
          "asset": {
            "type": "string",
            "enum": ["USD", "EUR", "GBP", "BTC", "ETH"]
          },
          "balance": {
            "type": "string",
            "pattern": "^[0-9]+(\\.[0-9]{1,8})?$"
          }
        }
      }
    },
    "state": {
      "type": "string",
      "enum": ["ACTIVE", "LOCKED", "INACTIVE"]
    }
  }
}
```

## Transformation Rules

### Transaction Transformation

#### Field Mappings
```yaml
source: id
target: transactionId
transform: "toLowerCase()"

source: amount
target: value
transform: "formatDecimal(8)"

source: currency
target: asset
transform: "identity()"

source: status
target: state
transform: "mapStatus()"

source: timestamp
target: createdAt
transform: "toUnixTimestamp()"

source: metadata
target: attributes
transform: "identity()"
```

#### Status Mapping
```yaml
PENDING: PENDING
COMPLETED: CONFIRMED
FAILED: REJECTED
CANCELLED: CANCELLED
```

### Account Transformation

#### Field Mappings
```yaml
source: accountId
target: walletId
transform: "generateWalletId()"

source: balances
target: holdings
transform: "transformBalances()"

source: status
target: state
transform: "mapAccountStatus()"
```

#### Status Mapping
```yaml
ACTIVE: ACTIVE
SUSPENDED: LOCKED
CLOSED: INACTIVE
```

## Validation Logic

### Pre-Transformation Validation

#### Transaction Validation
```javascript
function validateTransaction(source) {
  // Required fields check
  if (!source.id || !source.amount || !source.currency) {
    throw new Error("Missing required fields");
  }

  // Amount validation
  if (source.amount <= 0) {
    throw new Error("Invalid amount");
  }

  // Currency validation
  if (!["USD", "EUR", "GBP", "BTC", "ETH"].includes(source.currency)) {
    throw new Error("Invalid currency");
  }

  // Timestamp validation
  if (new Date(source.timestamp) > new Date()) {
    throw new Error("Future timestamp");
  }
}
```

#### Account Validation
```javascript
function validateAccount(source) {
  // Required fields check
  if (!source.accountId || !source.balances) {
    throw new Error("Missing required fields");
  }

  // Account ID format
  if (!/^ACC-[A-Z0-9]{8}$/.test(source.accountId)) {
    throw new Error("Invalid account ID format");
  }

  // Balances validation
  source.balances.forEach(balance => {
    if (balance.amount < 0) {
      throw new Error("Negative balance");
    }
  });
}
```

### Post-Transformation Validation

#### Transaction Validation
```javascript
function validateTransformedTransaction(target) {
  // Transaction ID format
  if (!/^[a-f0-9]{64}$/.test(target.transactionId)) {
    throw new Error("Invalid transaction ID format");
  }

  // Value format
  if (!/^[0-9]+(\.[0-9]{1,8})?$/.test(target.value)) {
    throw new Error("Invalid value format");
  }
}
```

#### Account Validation
```javascript
function validateTransformedAccount(target) {
  // Wallet ID format
  if (!/^[a-f0-9]{40}$/.test(target.walletId)) {
    throw new Error("Invalid wallet ID format");
  }

  // Holdings validation
  target.holdings.forEach(holding => {
    if (!/^[0-9]+(\.[0-9]{1,8})?$/.test(holding.balance)) {
      throw new Error("Invalid balance format");
    }
  });
}
```

## Error Handling

### Validation Errors
- **Type**: SchemaValidationError
- **Code**: 400
- **Action**: Log error, reject transformation
- **Notification**: Alert on repeated validation failures

### Transformation Errors
- **Type**: TransformationError
- **Code**: 500
- **Action**: Log error, retry transformation
- **Notification**: Alert on transformation failures

### Data Loss Prevention
- Backup of source data before transformation
- Checksum verification after transformation
- Audit trail of all transformations

## Revision History

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| 2024-04-19 | 1.0 | Initial draft | Technical Team | 