# CeciStyle Operations Platform

> Portfolio engineering showcase for a real-world Laravel business operations system.

CeciStyle Operations Platform is a full-stack business management system developed for an alterations and design business. It centralizes workflows that would otherwise be distributed across appointments, customer records, inventory, payments, garment intake, production planning, and physical garment tracking.

This repository is a **sanitized portfolio showcase**, not the production application. It documents selected architecture, engineering decisions, business rules, and representative code patterns without publishing customer data, credentials, production configuration, or the complete proprietary codebase.

## Engineering Focus

- PHP and Laravel application architecture
- MySQL relational data modeling with Eloquent
- Blade and JavaScript administrative workflows
- Role-based authorization and protected admin routes
- Appointment booking and availability workflows
- Inventory management and barcode-assisted lookup
- Square payment synchronization and payment-to-work matching
- Alteration orders and multi-garment production workflows
- Production milestones and workload allocation
- Garment location history and chain-of-custody tracking
- Media permission workflows
- Feature and regression testing with PHPUnit

## Selected Case Studies

| Case study | Engineering concepts demonstrated |
| --- | --- |
| [Booking & Scheduling](docs/case-studies/booking-and-scheduling.md) | Availability, validation, conflict prevention, multi-step workflows |
| [Authentication & Security](docs/case-studies/authentication-and-security.md) | Sessions, RBAC, middleware, throttling, employee account controls |
| [Inventory & Barcode Tracking](docs/case-studies/inventory-and-barcode-tracking.md) | Service layer, filtering, lifecycle controls, barcode-assisted operations |
| [Payments & Square](docs/case-studies/payments-and-square.md) | External payment synchronization, transactional business rules |
| [Alteration Operations](docs/case-studies/alteration-operations.md) | Domain workflow, database transactions, production planning, garment custody |
| [Customer Conflict Resolution](docs/case-studies/customer-conflict-strategy.md) | Strategy Pattern, identity resolution, concurrency, data integrity |

## Architecture

The production system follows Laravel conventions while organizing larger business capabilities into domain-oriented modules. Controllers coordinate HTTP workflows, Form Requests validate input, service classes encapsulate business logic, Eloquent models represent the domain, and migrations enforce relational integrity.

See [Architecture Overview](docs/architecture.md).

## Testing

The production repository's integrated alteration-operations release reported **53 passing tests and 219 assertions** when merged. This showcase does not claim independent coverage percentages and does not contain the complete production test suite.

See [Testing Strategy](docs/testing.md).

## Technology

PHP 8.2+ · Laravel 12 · MySQL · Blade · JavaScript · Vite · PHPUnit · Square SDK · AWS S3-compatible Laravel filesystem integration · DomPDF · html5-qrcode

## Repository Scope

The complete production repository remains private. This public-facing showcase is intentionally documentation-first and uses selected, sanitized examples rather than distributing a deployable copy of the business application.

No customer records, uploaded customer media, environment files, API keys, payment credentials, database dumps, or production secrets belong in this repository.

## Author

**Gean Vallejos**  
Full-Stack Web Developer — PHP, Laravel, MySQL & JavaScript
