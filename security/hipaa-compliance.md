# HIPAA Security Compliance Guide

This document outlines the HIPAA security compliance standards and automated checks for the Pixelated Empathy codebase.

## Overview

As a healthcare application handling Protected Health Information (PHI), Pixelated Empathy must adhere to HIPAA security standards, particularly those related to:

1. **Technical Safeguards**: Implementation of security measures to protect electronic PHI
2. **Access Controls**: Mechanisms to ensure appropriate access to PHI
3. **Audit Controls**: Systems to record and examine activity where PHI is accessed
4. **Integrity Controls**: Measures to prevent unauthorized alteration or destruction of PHI
5. **Transmission Security**: Protection of PHI when transmitted over a network

## Automated Security Checks

We provide automated tools to verify compliance with HIPAA security standards:

### 1. HIPAA Security Check Script

The `scripts/hipaa-security-check.js` script performs a comprehensive scan of the codebase for HIPAA security compliance issues:

```bash
# Run the script locally
node scripts/hipaa-security-check.js
```

This script checks for:

1. **Deprecated Crypto Methods**: Ensures use of secure encryption methods
2. **GCM Mode Encryption**: Verifies proper encryption algorithm (AES-256-GCM)
3. **Authentication Tags**: Confirms proper authentication for encrypted data
4. **Route Authentication**: Ensures all API routes have authentication checks
5. **PHI Audit Logging**: Verifies PHI handling includes audit logging

### 2. CI/CD Pipeline Integration

The security checks are integrated into our GitHub Actions workflow (`.github/workflows/security-scanning.yml`) to ensure ongoing compliance:

- Runs on every pull request and scheduled basis
- Generates a detailed HIPAA compliance report
- Fails the workflow if critical security issues are detected

## Security Standards

### Encryption Requirements

All PHI data must be encrypted using:

1. **AES-256-GCM** encryption algorithm
2. **Strong key derivation** using scrypt or PBKDF2
3. **Authentication tags** to ensure data integrity

Example of compliant encryption implementation:

```typescript
import crypto from 'crypto';

// Generate a secure key using proper key derivation
const key = crypto.scryptSync(password, salt, 32); // 256 bits

// Use AES-256-GCM with proper IV
const iv = crypto.randomBytes(16);
const cipher = crypto.createCipheriv('aes-256-gcm', key, iv);

// Encrypt the data
let encryptedData = cipher.update(data, 'utf8', 'hex');
encryptedData += cipher.final('hex');

// Get the authentication tag
const authTag = cipher.getAuthTag();
```

### Authentication and Authorization

All API routes must implement proper authentication and authorization:

```typescript
// ❌ Bad - No authentication
router.get('/patient-data', (req, res) => {
  // Direct access to patient data
});

// ✅ Good - With authentication
router.get('/patient-data', authenticate, authorize('doctor'), (req, res) => {
  // Access to patient data after authentication and authorization
});
```

### Audit Logging

All operations involving PHI must be logged for audit purposes:

```typescript
// ❌ Bad - No audit logging
function updatePatientRecord(patientId, data) {
  database.update('patients', patientId, data);
}

// ✅ Good - With audit logging
function updatePatientRecord(patientId, data, userId) {
  database.update('patients', patientId, data);
  auditLogger.log({
    action: 'UPDATE',
    resource: 'patient',
    resourceId: patientId,
    userId,
    timestamp: new Date(),
    changes: data
  });
}
```

## Troubleshooting Common Issues

### Deprecated Crypto Methods

If the security check reports "Deprecated/insecure crypto methods found":

1. Replace `createCipher` with `createCipheriv`
2. Replace `createDecipher` with `createDecipheriv`
3. Use a secure random IV for each encryption operation

### Missing Authentication

If the security check reports "Route without authentication checks":

1. Add the `authenticate` middleware to the route
2. Add appropriate authorization checks based on user roles
3. Verify token validation in the authentication middleware

### Missing Audit Logging

If the security check reports "PHI handling without audit logging":

1. Add audit logging to all functions that access or modify PHI
2. Log the user ID, action performed, and resource accessed
3. Include timestamps and relevant metadata in audit logs

## Manual Security Review Checklist

In addition to automated checks, perform regular manual reviews:

1. **Access Control Review**: Verify appropriate access to PHI
2. **Encryption Key Management**: Review key rotation and storage practices
3. **Authentication Mechanisms**: Evaluate strength of authentication methods
4. **Authorization Logic**: Review business logic for authorization decisions
5. **Audit Log Review**: Examine audit logs for suspicious activity

## Risk Assessment and Remediation

When security issues are identified:

1. **Categorize Risk**: Assess severity (Critical, High, Medium, Low)
2. **Document Finding**: Record details in the security tracking system
3. **Remediate Issue**: Implement fix according to best practices
4. **Verify Fix**: Re-run security check to confirm resolution
5. **Document Resolution**: Update security documentation as needed

## Questions and Support

If you have questions about HIPAA security compliance or encounter issues with the scanning tools, please contact the security team.