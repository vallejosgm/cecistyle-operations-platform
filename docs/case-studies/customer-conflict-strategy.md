# Case Study: Customer Conflict Resolution

## Problem

Public booking can receive an email and phone number that point to different existing customer records. Automatically overwriting or merging customer data would risk corrupting historical customer information.

![Customer identity conflict resolution](../images/05-customer-conflict-resolution.png)

## Design

The production application separates identity detection from conflict resolution.

During booking, customer identity is normalized and evaluated. If email and phone identify the same customer, that customer can be reused. Ambiguous combinations are recorded as an appointment customer conflict for staff review instead of silently modifying an existing record.

Conflict resolution uses the **Strategy Pattern**.

A common `CustomerConflictResolutionStrategy` contract defines the resolution operation. `CustomerConflictResolutionManager` selects one of four verified concrete strategies:

- `LinkEmailCustomerResolution`
- `LinkPhoneCustomerResolution`
- `CreateNewCustomerResolution`
- `UpdateExistingCustomerResolution`

The verified production structure is:

```text
app/Modules/Booking/CustomerConflict/
├── Contracts/CustomerConflictResolutionStrategy.php
├── Services/CustomerConflictResolutionManager.php
└── Strategies/
    ├── LinkEmailCustomerResolution.php
    ├── LinkPhoneCustomerResolution.php
    ├── CreateNewCustomerResolution.php
    └── UpdateExistingCustomerResolution.php
```

This allows each resolution behavior to own its validation and mutation rules while the caller works through a common interface.

See the sanitized implementation excerpts in [Selected Code Samples](../code-samples.md#strategy-pattern--customer-conflict-resolution).

## Concurrency and Data Integrity

Public booking creation uses a date-scoped cache lock and a database transaction. Availability is checked again inside the protected operation before the appointment is inserted.

Customer creation also handles unique-constraint races: if another request creates the same identity between lookup and insert, the resolver re-queries the matching records rather than blindly creating a duplicate.

## Testing

The production repository contains `tests/Unit/CustomerConflictResolutionManagerTest.php`. It constructs the manager with all four concrete strategies, verifies each supported mapping, and expects a `LogicException` for an unsupported resolution type.

A trimmed test excerpt is included in [Selected Code Samples](../code-samples.md#testing-the-strategy-selection).

## Engineering Value

This workflow demonstrates a practical use of a behavioral design pattern, separation of detection from resolution, defensive handling of ambiguous identity data, server-side validation, and concurrency-aware booking logic.

The complete production implementation remains private.