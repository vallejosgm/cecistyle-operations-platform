# Security Design

The production application uses layered application controls rather than relying on a single authorization check.

## Verified Controls

- Authenticated Laravel sessions protect administrative workflows.
- Role middleware restricts routes by staff role.
- Sensitive employee and configuration operations use narrower role boundaries.
- Public and account-recovery endpoints use route throttling.
- Administrative responses use a dedicated no-store middleware that sends `Cache-Control: no-store, no-cache, must-revalidate, max-age=0, private`, plus compatible `Pragma` and `Expires` headers.
- Form Requests provide server-side validation for business workflows.
- Database transactions protect multi-record operations.
- Payment matching uses row-level locking and ownership/status checks before paid work is authorized.
- Production secrets and customer data are intentionally excluded from this showcase.

## Authorization Boundary

The alteration operations module is protected by authenticated session middleware and the roles `admin`, `manager`, and `staff`. Other administrative capabilities, including employee management and selected settings/payment operations, use narrower role restrictions.

## Scope

This document describes controls verified in the private production repository. It is not a claim of formal security certification, penetration testing, or regulatory compliance.
