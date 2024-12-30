# Bitcoin Bridge Smart Contract

A secure and robust smart contract that enables trustless cross-chain transfers between Stacks and Bitcoin networks.

## Overview

The Bitcoin Bridge Smart Contract facilitates secure asset transfers between Stacks and Bitcoin blockchains through a multi-validator consensus system. It implements comprehensive security measures, deposit validation, confirmation thresholds, and emergency controls.

## Features

- **Multi-Validator Consensus**: Secure validation system for cross-chain transfers
- **Deposit Management**: Safe handling of user deposits with confirmation thresholds
- **Withdrawal Controls**: Secure withdrawal mechanism with balance tracking
- **Emergency Controls**: Administrative functions for emergency situations
- **Balance Tracking**: Accurate tracking of user balances and total bridged amounts
- **Security Measures**: Comprehensive validation for addresses, transactions, and signatures

## Contract Architecture

### Core Components

1. **Trait Definitions**

   - `bridgeable-token-trait`: Interface for token transfer and balance operations

2. **State Management**

   - Bridge status tracking
   - Total bridged amount monitoring
   - Transaction height tracking
   - User balance management

3. **Data Structures**
   - Deposits mapping
   - Validator registry
   - Signature verification
   - Balance tracking

### Security Features

- Row-level security
- Multi-signature validation
- Amount thresholds
- Address validation
- Transaction verification
- Emergency controls

## Functions

### Administrative Functions

```clarity
(define-public (initialize-bridge))
(define-public (pause-bridge))
(define-public (resume-bridge))
```

Controls bridge operations and emergency measures.

### Validator Management

```clarity
(define-public (add-validator (validator principal)))
(define-public (remove-validator (validator principal)))
```

Manages the validator set for transaction verification.

### Bridge Operations

```clarity
(define-public (initiate-deposit (tx-hash (buff 32)) (amount uint) (recipient principal) (btc-sender (buff 33))))
(define-public (confirm-deposit (tx-hash (buff 32)) (signature (buff 65))))
(define-public (withdraw (amount uint) (btc-recipient (buff 34))))
(define-public (emergency-withdraw (amount uint) (recipient principal)))
```

Core functions for deposit and withdrawal operations.

### Read-Only Functions

```clarity
(define-read-only (get-deposit (tx-hash (buff 32))))
(define-read-only (get-bridge-status))
(define-read-only (get-validator-status (validator principal)))
(define-read-only (get-bridge-balance (user principal)))
```

Query functions for contract state and user information.

## Configuration

### Constants

```clarity
MIN-DEPOSIT-AMOUNT: u100000
MAX-DEPOSIT-AMOUNT: u1000000000
REQUIRED-CONFIRMATIONS: u6
```

### Error Codes

- `ERROR-NOT-AUTHORIZED` (u1000): Unauthorized access attempt
- `ERROR-INVALID-AMOUNT` (u1001): Amount outside valid range
- `ERROR-INSUFFICIENT-BALANCE` (u1002): Insufficient funds
- `ERROR-INVALID-BRIDGE-STATUS` (u1003): Invalid bridge state
- `ERROR-INVALID-SIGNATURE` (u1004): Invalid signature
- `ERROR-ALREADY-PROCESSED` (u1005): Duplicate transaction
- `ERROR-BRIDGE-PAUSED` (u1006): Bridge is paused
- `ERROR-INVALID-VALIDATOR-ADDRESS` (u1007): Invalid validator
- `ERROR-INVALID-RECIPIENT-ADDRESS` (u1008): Invalid recipient
- `ERROR-INVALID-BTC-ADDRESS` (u1009): Invalid BTC address
- `ERROR-INVALID-TX-HASH` (u1010): Invalid transaction hash
- `ERROR-INVALID-SIGNATURE-FORMAT` (u1011): Malformed signature

## Usage

### Initiating a Deposit

1. Validator verifies the Bitcoin transaction
2. Calls `initiate-deposit` with transaction details
3. Other validators confirm the deposit
4. Upon reaching confirmation threshold, funds are credited

### Withdrawing Funds

1. User calls `withdraw` with amount and Bitcoin address
2. Contract verifies balance and validity
3. Emits withdrawal event for processing
4. Updates user balance and total bridged amount

## Security Considerations

- **Validator Security**: Only authorized validators can initiate and confirm deposits
- **Amount Limits**: Enforced minimum and maximum deposit amounts
- **Address Validation**: Strict validation for all addresses and transaction hashes
- **Multi-Signature**: Required confirmations prevent single-validator attacks
- **Emergency Controls**: Admin functions for critical situations
- **Balance Protection**: Strict balance checking and updates

## Development

### Prerequisites

- Clarity language knowledge
- Understanding of Bitcoin and Stacks networks
- Familiarity with cross-chain bridges

### Testing

Comprehensive testing should cover:

- Deposit validation
- Withdrawal processes
- Validator management
- Emergency procedures
- Edge cases and error conditions
