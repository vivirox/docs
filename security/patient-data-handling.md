# Patient Data Security Guidelines

## Patient ID Handling

Patient IDs are considered Protected Health Information (PHI) under HIPAA regulations and must be handled with strict security measures. This document outlines the proper procedures for working with patient identifiers in the Gradiant system.

## Key Requirements

1. **Use UUID Format**: All patient IDs must use the standard UUID format (8-4-4-4-12 hexadecimal characters).
2. **Validate Formats**: All patient IDs must be validated before processing.
3. **Mask Before Logging**: Never log raw patient IDs; always use the provided masking functions.
4. **Encrypt Before Storage**: Patient IDs must be encrypted before being stored in any database.
5. **Audit All Access**: All access to patient data must be recorded in the audit log.

## Implementation Details

### UUID Validation

Patient IDs must be validated using the following pattern:

```typescript
/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i
```

All APIs accepting patient IDs should use Zod or similar validation:

```typescript
const patientIdSchema = z.string().uuid({ message: "Patient ID must be a valid UUID" });
```

### ID Masking for Logs and Notifications

When displaying or logging patient IDs, always use the masking function:

```typescript
// For UUID format (8-4-4-4-12)
// Input:  '123e4567-e89b-12d3-a456-426614174000'
// Output: '123e4567...4000'
```

### Encryption for Storage

Before storing patient IDs in databases or sending via APIs, encrypt them:

```typescript
// Use the security service
const encryptedId = await encryptMessage(patientId);
```

### Audit Logging

All operations involving patient data must be logged using the audit system:

```typescript
// Log patient data access with masked ID
auditLogger.info(`Patient data ${action}`, {
  patientId: maskedPatientId,
  action,
  timestamp: new Date().toISOString(),
  ...additionalDetails
});
```

## External Communications

1. **Slack/Chat Integration**: Never send raw patient IDs to external systems like Slack. Always use masked IDs.
2. **Email**: Patient IDs should never be included in email subjects and should be masked in the body.
3. **Reports**: When generating reports containing patient IDs, ensure proper authorization and use masked IDs where possible.

## PII Detection

Our system includes automated PII detection for patient IDs in various formats:

1. UUID format: `/\b[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}\b/gi`
2. Labeled format: `/\bPatient ID:?\s*[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}\b/gi`
3. Numeric format: `/\bPatient ID:?\s*\d+\b/gi`

## Security Incident Response

If patient IDs are inadvertently exposed:

1. Document the incident immediately
2. Notify the security and compliance teams
3. Assess the scope of the exposure
4. Implement containment measures
5. Complete an incident report

## Developer Checklist

When working with code that processes patient data:

- [ ] Validate all patient ID inputs using UUID format
- [ ] Use masking functions for any logging or display
- [ ] Encrypt patient IDs before storage
- [ ] Implement audit logging for all patient data access
- [ ] Run security tests to verify proper handling
- [ ] Document any special handling in code comments
- [ ] Review external communications for inadvertent exposure

## Related Security Measures

For complete security coverage, also reference:

- [Encryption Guidelines](./encryption.md)
- [HIPAA Compliance Policy](./hipaa-compliance.md)
- [Audit Logging Standards](./audit-logging.md)
- [PII Detection System](./pii-detection.md)

## Regulatory Requirements

This policy is designed to comply with:

- HIPAA Privacy Rule
- HIPAA Security Rule
- GDPR (where applicable)
- State-level privacy regulations

Last Updated: May 2023 