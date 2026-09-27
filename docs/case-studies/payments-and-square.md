# Case Study: Payments & Square

## Problem

A payment appearing in the payment system does not automatically identify which alteration work it authorizes. The application therefore needs to synchronize payment records and explicitly associate available payment value with selected work.

## Verified Business Logic

The Alteration Operations service performs payment matching inside a database transaction. Selected `GarmentAlteration` rows are constrained to the current order and loaded with `lockForUpdate()` before their payment state can change.

Before creating a payment batch, the service verifies that:

- every selected job belongs to the current alteration order;
- only pending jobs can be matched;
- the Square transaction is not already linked to another alteration order;
- previously matched value is included when calculating the remaining amount; and
- the selected work does not exceed the payment value still available.

After a valid match, a `PaymentBatch` is recorded, the selected jobs are marked paid, and the alteration order moves into production.

See the sanitized transaction and row-lock excerpt in [Selected Code Samples](../code-samples.md#payment-matching--transaction--row-lock).

## Engineering Concepts

- third-party payment integration;
- local transaction records;
- database transactions;
- row locking for concurrent updates;
- validation of cross-record ownership;
- prevention of duplicate/reused payment allocation; and
- business-state transitions driven by payment status.

The showcase describes these rules without exposing Square credentials or the complete production integration.