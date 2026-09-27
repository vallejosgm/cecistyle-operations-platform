# Case Study: Alteration Operations

## Problem

An alterations business must coordinate more than a customer and an appointment. A single order can contain multiple garments, each garment can contain multiple jobs, paid work must be distinguished from pending work, production effort must be distributed across fitting milestones, and the physical garment must remain traceable while it moves through the shop.

<table>
  <tr>
    <td width="50%"><img src="../images/06-alteration-order.png" alt="Alteration order"></td>
    <td width="50%"><img src="../images/08-garment-work-ticket.png" alt="Garment work ticket"></td>
  </tr>
</table>

## Solution

The Alteration Operations module models this workflow as related domain records rather than a single status field.

The verified production implementation includes alteration orders, garments, alteration jobs, payment batches, garment locations, garment location events, production milestones, media permission records, and garment media.

## Key Business Rules

### Atomic order creation

Order creation is executed inside a database transaction. The workflow resolves or creates the customer, creates the order, creates each garment, records its initial physical location, creates production milestones, and creates the requested alteration jobs.

### Paid-work authorization

Payment matching locks the selected alteration records for update and verifies that:

- every selected job belongs to the current order;
- only pending jobs can be matched;
- a payment is not reused by another alteration order; and
- the selected work does not exceed the available payment amount.

After a successful match, the selected work is marked paid and the order moves into production. A sanitized excerpt is available in [Selected Code Samples](../code-samples.md#payment-matching--transaction--row-lock).

### Production milestones

Workload can be allocated across try-on and pickup milestones. This makes the production calendar represent portions of the garment's workload rather than duplicating the entire workload at every appointment.

![Production capacity calendar](../images/07-production-calendar.png)

### Garment chain of custody

Garments have a current physical location and a separate location-event history. A move records the previous location, destination, responsible user, timestamp, and optional note. Moving to operational locations can also update garment status, such as in progress, ready, or released.

## Why This Matters

This module demonstrates transactional business logic, relational modeling, concurrency-aware payment matching, auditable state transitions, and a workflow designed around real operational constraints rather than generic CRUD screens.

## Representative Production Components

```text
app/Modules/Alterations/Controllers/
app/Modules/Alterations/Requests/
app/Modules/Alterations/Services/AlterationOrderService.php
app/Models/AlterationOrder.php
app/Models/Garment.php
app/Models/GarmentAlteration.php
app/Models/GarmentLocationEvent.php
app/Models/GarmentProductionMilestone.php
routes/alterations.php
tests/Feature/AlterationOperationsTest.php
```

The complete source remains in the private production repository.