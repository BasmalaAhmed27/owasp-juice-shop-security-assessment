# Finding 03 — Mass Assignment Leading to Unauthorized Administrative Privileges

## Classification
Mass Assignment / Over-Posting

## Context
- Standard user account
- User-controlled JSON request body
- Backend automatically processes supplied object properties

## Security Requirement
Users should only be able to modify explicitly permitted account properties.

Sensitive server-managed attributes such as roles, permissions, or internal authorization flags must not be directly writable through user-controlled request bodies.

## Test Methodology
1. Captured a user-related request using Burp Suite.
2. Reviewed the JSON request body and the properties accepted by the backend.
3. Added a privileged property to the request body.
4. Set the privileged property to a value associated with administrative access.
5. Sent the modified request to the backend.
6. Refreshed or re-authenticated the account to verify whether the supplied property had been persisted.

## Expected Result
The backend should reject or ignore properties that are not explicitly intended to be user-modifiable.

Sensitive properties such as authorization roles should be controlled exclusively by the server.

## Observed Result
The backend accepted the additional privileged property supplied in the request body and bound it to the user object.

The modified privilege value was persisted by the application.

## Validation
After refreshing or re-authenticating the account, the account was recognized with administrative privileges.

This confirmed that the server accepted and persisted a security-sensitive property controlled by the client.

## Impact
A standard user can modify a privileged server-side property and escalate their account privileges.

This can result in unauthorized administrative access and compromise of the application's authorization model.

## Remediation
The backend should use an explicit allowlist of properties that users are permitted to modify.

Sensitive fields such as:

- roles
- permissions
- administrative flags
- internal account state

should never be bound directly from untrusted client input.

The application should map approved request fields individually rather than automatically assigning all submitted properties to the backend model.
