# Credential and PHI Management Guide

This document outlines best practices for handling credentials and Protected Health Information (PHI) in the Gradiant Ascent codebase.

## Overview

As a healthcare application, Gradiant Ascent must maintain stringent security standards, particularly regarding:

1. **Credentials**: API keys, client IDs, secrets, tokens
2. **Protected Health Information (PHI)**: Patient identifiers, medical record numbers, etc.

This guide provides tools and practices to prevent accidental exposure of sensitive data.

## Tools Provided

### 1. Credential Cleanup Script

We provide a script (`scripts/clean-credentials.js`) to detect and remediate hardcoded credentials:

```bash
# To check for hardcoded credentials without modifying files
node scripts/clean-credentials.js --check-only

# To automatically replace hardcoded credentials with environment variables
node scripts/clean-credentials.js
```

The script:
- Scans for hardcoded credentials and PHI in code files
- Replaces direct values with environment variable references
- Provides special handling for documentation files
- Generates reports of findings

### 2. GitLeaks Configuration

The `.gitleaks.toml` file is configured to detect and prevent accidental credential commits. It:
- Scans for various credential patterns
- Applies appropriate allowlists for test files and example values
- Functions as a pre-commit check in CI/CD pipelines

## Best Practices

### Using Environment Variables

Always use environment variables for sensitive values:

```typescript
// ❌ Bad - hardcoded credentials
const clientId = "example-client-id";

// ✅ Good - environment variables with fallback for development
const clientId = process.env.CLIENT_ID || "example-client-id";
```

### Documentation Examples

When showing code examples in documentation:

```mdx
// ❌ Bad - realistic-looking credentials
ANTHROPIC_API_KEY=YOUR_API_KEY_HERE

// ✅ Good - obvious placeholder
ANTHROPIC_API_KEY=YOUR_API_KEY_HERE
```

### PHI Handling

For PHI data:

1. Never hardcode real patient identifiers
2. Use environment variables or secure data stores
3. Mark PHI fields clearly in type definitions

```typescript
interface PatientRecord {
  // Clearly marked as PHI
  /** @PHI - Patient identifier */
  patientId: string;

  // Non-PHI data
  recordCount: number;
}
```

## Pre-Commit and CI/CD Validation

Our GitHub Actions workflows include:

1. Secret scanning using GitLeaks
2. PHI detection using specialized tools
3. Automated credential cleanup checks

When these scans identify potential issues:
1. Review the generated reports
2. Run the cleanup script if appropriate
3. Update allowlists for legitimate false positives

## Required Environment Variables

The following environment variables should be defined in your `.env` file (and added to deployment environments):

```
# API Keys
API_KEY=YOUR_API_KEY_HERE

# OAuth Credentials
CLIENT_ID=your_client_id_here
CLIENT_SECRET=your_client_secret_here

# Development PHI
PATIENT_ID=example-patient-id
```

## Questions and Support

If you have questions about credential security or encounter issues with the scanning tools, please contact the security team.