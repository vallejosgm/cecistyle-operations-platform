# Case Study: Inventory & Barcode Tracking

## Problem

Physical sewing and alterations inventory needs reliable identification, searchable categorization, location tracking, photos, and controlled retirement without deleting operational history.

![Inventory item with permanent code and barcode](../images/04-inventory-barcode.png)

## Implementation

The production Inventory module separates HTTP concerns from business logic. `InventoryController` receives `InventoryService` and `InventoryFilterService` through dependency injection. Item creation and updates are delegated to the service layer, while filtering remains reusable and separate from controller orchestration.

`InventoryService` coordinates database records, permanent inventory codes, uploaded images, and activity history. Creation and update operations use transaction boundaries. If an exception occurs after new image files have been stored, the database transaction is rolled back and the newly stored files are cleaned up before the original exception is propagated.

Role-aware workflows allow staff to work with inventory while manager/admin users can review pending retirement requests and manage inventory configuration. Barcode endpoints support label generation and barcode-assisted item lookup as part of the physical workflow.

A representative transaction/failure-handling excerpt is included in [Selected Code Samples](../code-samples.md#inventory--databasefile-consistency).

## Engineering Value

This module demonstrates:

- dependency injection and service-layer separation;
- reusable Eloquent filtering logic;
- database/file consistency across failure paths;
- permanent operational identifiers;
- activity/audit metadata;
- role-aware lifecycle transitions rather than destructive deletion; and
- integration of physical labels/scanning with a web application.

The complete production implementation is intentionally not reproduced in this showcase.