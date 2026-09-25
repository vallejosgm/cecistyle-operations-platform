# Selected Code Samples

These small excerpts are included to demonstrate implementation style without publishing the complete production application.

## Strategy Pattern — Customer Conflict Resolution

The booking workflow can detect cases where submitted email and phone data point to different existing customers. Resolution behavior is selected through a common strategy contract.

```php
interface CustomerConflictResolutionStrategy
{
    public function resolve(
        Request $request,
        AppointmentCustomerConflict $conflict
    ): Customer;
}
```

The manager maps the requested resolution to one of four concrete strategies:

```php
return match ($resolutionType) {
    'linked_email_customer' => $this->linkEmailCustomerResolution,
    'linked_phone_customer' => $this->linkPhoneCustomerResolution,
    'created_new_customer' => $this->createNewCustomerResolution,
    'updated_existing_customer' => $this->updateExistingCustomerResolution,
    default => throw new LogicException(
        'Unsupported customer conflict resolution.'
    ),
};
```

A dedicated PHPUnit unit test verifies all four mappings and the unsupported-resolution failure case.

## Role-Based Route Middleware

A compact middleware enforces the role boundary before the request reaches protected application logic.

```php
$user = $request->user();

abort_unless($user && $user->hasRole($roles), 403);

return $next($request);
```

## Payment Matching Integrity

The alteration workflow performs payment matching inside a database transaction and locks the selected work rows while validating ownership, payment state, previous transaction use, and available payment amount before changing work to paid status.

This is intentionally summarized rather than reproducing the complete production service.

## Inventory Transaction Boundary

Inventory creation and updates coordinate database records, uploaded images, audit activity, and cleanup behavior. If a database operation fails after files have been stored, newly stored files are cleaned up before the exception is propagated.

This is intentionally summarized to show the engineering approach without publishing the complete service implementation.

## Showcase Scope

The complete source remains in a private production repository. Samples here are selected specifically to demonstrate architecture and engineering decisions without exposing credentials, customer records, proprietary implementation detail, or a deployable copy of the application.
