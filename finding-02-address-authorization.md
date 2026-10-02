# Finding 02 — Unauthorized Deletion of Another User's Address

## Classification
Broken Object Level Authorization (BOLA) / IDOR

## Context
- Account A
- Account B
- Both accounts are independent
- Each user can create and manage their own saved addresses

## Security Requirement
A user should only be able to modify or delete address records that belong to their own account.

## Test Methodology
1. Created an address under Account B.
2. Identified the address ID belonging to Account B.
3. Authenticated as Account A.
4. Sent a deletion request targeting Account B's address ID.
5. Used Account A's authenticated session for the request.
6. Logged back into Account B to verify whether the address still existed.

## Expected Result
The server should reject the request because Account A does not own or have authorization to manage Account B's address.

## Observed Result
The server processed the deletion request successfully even though the target address belonged to Account B.

## Validation
After logging back into Account B, the targeted address was no longer present.

This confirmed that Account A had successfully deleted another user's saved address.

## Impact
An authenticated user can delete address records belonging to another independent user.

This leads to unauthorized modification of another user's account data and affects data integrity and availability.

## Remediation
The backend should enforce object-level authorization checks before processing address modification or deletion requests.

The server should verify that the address being accessed belongs to the authenticated user before performing any action.

Client-supplied object identifiers should never be trusted as proof of ownership.
