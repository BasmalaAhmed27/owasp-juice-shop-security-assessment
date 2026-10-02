# Finding 01 — Unauthorized Modification and Deletion of Another User's Basket Item

## Classification
Broken Object Level Authorization (BOLA) / IDOR

## Context
- Account A
- Account B
- Both accounts are independent
- Each user has their own basket containing BasketItems

## Security Requirement
A user should only be able to modify or delete BasketItems that belong to a basket they are authorized to manage.

## Test Methodology
1. Created a BasketItem under Account B.
2. Identified the BasketItem ID belonging to Account B.
3. Authenticated as Account A.
4. Sent a request targeting Account B's BasketItem.
5. Modified the quantity of the BasketItem using Account A's session.
6. Repeated the test using the delete functionality.
7. Logged back into Account B to verify the results.

## Expected Result
The server should reject requests from Account A attempting to modify or delete BasketItems owned by Account B.

## Observed Result
The server accepted the requests sent from Account A and allowed modification of Account B's BasketItem.

Account A was also able to delete the BasketItem belonging to Account B.

## Validation
After logging back into Account B:

- The BasketItem quantity had changed.
- The BasketItem targeted by the deletion request had been removed.

This confirmed that the changes were persisted server-side.

## Impact
An authenticated user can modify or delete basket items belonging to another user.

This results in unauthorized manipulation of another user's shopping basket and affects the integrity of user-controlled data.

## Remediation
The backend should enforce object-level authorization checks on every BasketItem operation.

Before allowing update or deletion, the server should verify that:

- The authenticated user owns or is authorized to manage the related basket.
- The targeted BasketItem belongs to that authorized basket.

Authorization decisions should be enforced server-side rather than relying on identifiers supplied by the client.
