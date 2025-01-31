# Medical Device Tracking Smart Contract

A Clarity smart contract for transparent tracking of medical devices throughout their lifecycle, managing certifications, and maintaining regulatory compliance on the Stacks blockchain.

## Overview

This smart contract enables medical device manufacturers, healthcare facilities, and regulatory bodies to:
- Track the complete lifecycle of medical devices
- Manage and verify device certifications
- Maintain an immutable history of device status changes
- Ensure regulatory compliance through authorized certification bodies

## Features

### Device Lifecycle Management
- Register new medical devices
- Track device status changes through multiple stages
- Maintain detailed history of all status changes
- Support for multiple device owners/operators

### Certification Management
- Support for major medical certifications (FDA, CE, ISO, Safety)
- Authorized regulatory body system
- Certification verification
- Certificate revocation capabilities

### Status Tracking
Devices can be in one of four states:
1. `MANUFACTURED` (u1) - Initial production complete
2. `TESTING` (u2) - Under testing/validation
3. `DEPLOYED` (u3) - In active use
4. `MAINTAINED` (u4) - Under maintenance/service

### Certification Types
The contract supports four certification types:
1. `FDA` (u1) - Food and Drug Administration certification
2. `CE` (u2) - European Conformity marking
3. `ISO` (u3) - International Organization for Standardization
4. `SAFETY` (u4) - General safety certification

## Contract Functions

### Administrative Functions

```clarity
(add-regulatory-body (authority principal) (cert-type uint))
```
Adds a new authorized regulatory body. Only callable by contract owner.

### Device Management

```clarity
(register-device (device-id uint) (initial-status uint))
```
Registers a new device with an initial status.

```clarity
(update-device-status (device-id uint) (new-status uint))
```
Updates the status of an existing device.

### Certification Management

```clarity
(add-certification (device-id uint) (cert-type uint))
```
Adds a certification to a device. Only callable by authorized regulatory bodies.

```clarity
(revoke-certification (device-id uint) (cert-type uint))
```
Revokes an existing certification. Only callable by the issuing body or contract owner.

### Read-Only Functions

```clarity
(verify-certification (device-id uint) (cert-type uint))
```
Verifies if a specific certification is valid for a device.

```clarity
(get-device-history (device-id uint))
```
Returns the complete status history of a device.

```clarity
(get-device-status (device-id uint))
```
Returns the current status of a device.

```clarity
(get-certification-details (device-id uint) (cert-type uint))
```
Returns detailed information about a specific certification.

## Error Codes

- `ERR_UNAUTHORIZED` (u1) - Caller not authorized for the operation
- `ERR_INVALID_DEVICE` (u2) - Invalid device ID
- `ERR_STATUS_UPDATE_FAILED` (u3) - Status update operation failed
- `ERR_INVALID_STATUS` (u4) - Invalid status code provided
- `ERR_INVALID_CERTIFICATION` (u5) - Invalid certification type
- `ERR_CERTIFICATION_EXISTS` (u6) - Certification already exists

## Implementation Guide

### Prerequisites
- Stacks blockchain environment
- Clarity contract deployment tools
- Required permissions for contract deployment

### Deployment Steps

1. Deploy the contract to the Stacks blockchain:
```bash
clarinet contract deploy medical-device
```

2. Initialize regulatory bodies:
```clarity
(contract-call? .medical-device add-regulatory-body 'SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7 u1)
```

3. Register initial devices:
```clarity
(contract-call? .medical-device register-device u1 u1)
```

### Usage Example

```clarity
;; Register a new device
(contract-call? .medical-device register-device u1 DEVICE_STATUS_MANUFACTURED)

;; Update device status
(contract-call? .medical-device update-device-status u1 DEVICE_STATUS_TESTING)

;; Add FDA certification
(contract-call? .medical-device add-certification u1 CERT_TYPE_FDA)

;; Verify certification
(contract-call? .medical-device verify-certification u1 CERT_TYPE_FDA)
```

## Security Considerations

1. **Access Control**
   - Only authorized regulatory bodies can issue certifications
   - Only device owners or contract owner can update device status
   - Certification revocation limited to issuer or contract owner

2. **Data Validation**
   - All device IDs must be valid
   - All status codes must be valid
   - All certification types must be valid

3. **State Management**
   - Device history limited to last 10 status changes
   - Immutable certification records
   - Non-reusable device IDs

## Best Practices

1. **Device Registration**
   - Register devices immediately after manufacturing
   - Include appropriate initial status
   - Verify device ID uniqueness

2. **Status Updates**
   - Update status promptly when changes occur
   - Maintain accurate status progression
   - Document reason for status changes

3. **Certification Management**
   - Obtain required certifications before deployment
   - Regular certification validity checks
   - Prompt revocation of invalid certifications

## Contributing

We welcome contributions to improve the contract. Please follow these steps:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request