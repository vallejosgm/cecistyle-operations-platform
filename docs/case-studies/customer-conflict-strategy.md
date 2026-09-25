# Case Study: Customer Conflict Resolution

## Problem

Public booking can receive an email and phone number that point to different existing customer records. Automatically overwriting or merging customer data would risk corrupting historical customer information.

## Design

The production application separates identity detection from conflict resolution.

During booking, customer identity is normalized and evaluated. If email and phone identify the same customer, that customer can be reused. Ambiguous combinations are recorded as an appointment customer conflict for staff review instead of silently modifying an existing record.

Conflict resolution uses the **Strategy Pattern**.

A common `CustomerConflictResolutionStrategy` contract defines the resolution operation. A `CustomerConflictResolutionManager` selects one of four verified strategies:

- `LinkEmailCustomerResolution`
- `LinkPhoneCustomerResolution`
- `CreateNewCustomerResolution`
- `UpdateExistingCustomerResolution`

This allows each resolution behavior to own its validation and mutation rules while the caller works through a common interface.

## Concurrency and Data Integrity

Public booking creation uses a date-scoped cache lock and a database transaction. Availability is checked again inside the protected operation before the appointment is inserted.

Customer creation also handles unique-constraint races: if another request creates the same identity between lookup and insert, the resolver re-queries the matching records rather than blindly creating a duplicate.

## Testing

The production repository contains a unit test for `CustomerConflictResolutionManager` that verifies selection of all four strategies and rejection of unsupported resolution types.

## Engineering Value

This workflow demonstrates a practical use of a behavioral design pattern, separation of detection from resolution, defensive handling of ambiguous identity data, server-side validation, and concurrency-aware booking logic.

The complete production implementation remains private.
