# Raid POS Mailchimp Connector v3.00.01 Release Notes
**Release Date:** August 24th, 2026

_This release fixes several sync crashes that could block item, customer, or order syncing, and makes order and customer syncing more reliable for busy stores._

## New Features & Improvements

### More reliable order and sales syncing
Order and sales syncing now works through a manageable batch of orders at a time instead of trying to handle everything in one pass, so stores with a lot of sales activity sync more reliably.

- A sale that fails to sync is now automatically retried after a short cooldown instead of getting stuck.
- Customer lifetime spend and order counts shown in Mailchimp are kept up to date more efficiently.

### Smoother customer syncing for stores with a lot of history
Improved how new customer numbers are assigned when importing customers from Mailchimp, so performance stays consistent even for stores that have been syncing for a long time.

- Customer syncing now paces its requests to Mailchimp to avoid hitting Mailchimp's usage limits during large syncs.

## Bug Fixes

### Fixed item syncing crashing for an entire catalog
Item syncing could crash for every item in a store's catalog when certain vendor or category settings didn't line up, blocking item sync entirely. This is now fixed.

### Fixed customer syncing breaking after certain configuration changes
After certain configuration changes, customer syncing could start failing with an error until it was corrected by hand. The connector now repairs this automatically during install/upgrade, so customer sync keeps working without manual intervention.

### Fixed order-total syncing crashing for stores with many customers
Syncing customers' order totals could fail outright for stores with a large customer base. This is now fixed.

### Fixed customer syncing crashes for specific customer records
- Fixed a crash when a customer had no last name on file.
- Fixed a crash caused by a specific address field configuration.
- A problem with one customer record no longer stops the rest of that sync cycle's customers from syncing.

### Fixed item syncing crashing for brand-new items with no inventory
A brand-new item that hadn't yet been assigned inventory at any location could cause item syncing to crash. This is now fixed.
