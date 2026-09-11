# MailChimp Connector v3.00.14 Release Notes
**Release Date:** September 13, 2026

This release fixes a range of issues that could interrupt customer, item, and order syncing to MailChimp, and speeds up how quickly customer spending data updates.

## New Features & Improvements

### Customer Spend and Order Totals Update Faster
A customer's lifetime spend and order count in MailChimp now update right after that customer syncs, instead of waiting on a separate batch job to run afterward.

## Bug Fixes

### Accounts with Longer Names Could Have Sync Settings Fail Silently
Fixed an issue where certain sync settings failed to apply for accounts with names over 20 characters.

### Item Sync Permanently Skipped Discontinued Items
Fixed an issue where an item that had already synced to MailChimp, but was later discontinued, stopped receiving updates entirely - even corrections to its existing listing.

### Customers with Only a Phone Number Failed to Sync for Text Marketing
Fixed several issues that could stop a customer with a phone number but no email from syncing to MailChimp for SMS marketing.

* A conflict with an existing MailChimp contact could make the sync fail outright instead of updating the right record.
* A crash could happen when syncing a customer with a phone number but no email on file.
* Text marketing consent status now syncs to MailChimp correctly, instead of sometimes being rejected or reset to the wrong value.

### Bounced Email Addresses Weren't Recognized Correctly
Fixed an issue where a customer whose email MailChimp flagged as undeliverable (bounced) wasn't reflected correctly in Counterpoint. The connector no longer treats that customer as subscribed, and won't keep trying to email an address MailChimp already flagged as bad.

### Some Customers Repeatedly Failed to Sync with No Way to Recover
Fixed a group of issues where a customer whose MailChimp record had been archived, or flagged for a compliance-related reason, failed the same way on every sync cycle with no resolution.

* These customers are now recognized and marked so they stop being retried endlessly.
* An unexpected or malformed response from MailChimp's servers no longer crashes the sync process - the actual problem is now logged clearly instead.
* A failed item lookup no longer causes the connector to try creating a duplicate item in MailChimp's store.

### Orders Could Sync to MailChimp with Missing or Invalid Items
Fixed an issue where an order could reach MailChimp missing one or more line items, with no sign anything was wrong. An order referencing an item that hasn't finished syncing to MailChimp's store yet is now held and retried instead of being sent incomplete.

### Some Sales Tickets Were Never Picked Up for Order Syncing
Fixed an issue where certain sales tickets for customers already set up for MailChimp syncing were never picked up by order syncing at all. A one-time correction also picks up the affected historical tickets.

### An Item Number Containing a Percent Sign Failed to Sync
Fixed an issue where an item number containing a percent sign would fail to sync to MailChimp every time, with no way to succeed.

### Crash Placing an Order for a Phone-Only Customer
Fixed a crash that could happen when an order was placed for a customer with only a phone number (no email) on file.
