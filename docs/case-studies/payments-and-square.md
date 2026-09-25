# Case Study: Payments & Square

## Problem

A payment appearing in the payment system does not automatically identify which alteration work it authorizes. The application therefore needs to synchronize payment records and explicitly associate available payment value with selected work.

## Verified Business Logic

The Alteration Operations service performs payment matching inside a database transaction and locks selected alteration records for update.

Before creating a payment batch it verifies ownership, pending status, prior use of the transaction, the amount already matched, and the remaining available payment amount. Selected work cannot be authorized beyond the value available from the payment.

After a valid match, the payment batch is recorded, the selected jobs are marked paid, and the alteration order can move into production.

## Engineering Concepts

- third-party payment integration;
- local transaction records;
- database transactions;
- row locking for concurrent updates;
- validation of cross-record ownership;
- prevention of duplicate/reused payment allocation; and
- business-state transitions driven by payment status.

The showcase describes these rules without exposing Square credentials or the complete production integration.
