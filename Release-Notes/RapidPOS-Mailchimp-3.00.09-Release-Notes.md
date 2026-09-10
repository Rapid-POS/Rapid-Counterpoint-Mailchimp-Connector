# RapidPOS Connector for MailChimp v3.00.10 Release Notes
**Release Date:** September 13, 2026

Fixes for SMS-only contact syncing, bounced email handling, and Message Center alerts.

## New Features & Improvements

### More Reliable Initial Marketing Preferences
A new customer's initial email/SMS marketing preference is now set correctly from the moment the record is created.

## Bug Fixes

### Crash Importing SMS-Only Contacts from MailChimp
Fixed a crash that happened when importing MailChimp contacts that have only a phone number and no email address.

### Customers with Only a Phone Number Failed to Sync for Text Marketing
Fixed several issues that could stop a customer with a phone number but no email from syncing to MailChimp for SMS marketing.

* A conflict with an existing MailChimp contact could make the sync fail outright instead of updating the right record.
* A separate crash could happen when syncing a customer with a phone number but no email on file.
* Text marketing consent status now syncs to MailChimp correctly, instead of sometimes being rejected or reset to the wrong value.

### Bounced Email Addresses Weren't Recognized Correctly
Fixed an issue where a customer whose email MailChimp flagged as undeliverable (bounced) wasn't reflected correctly in Counterpoint. The connector no longer treats that customer as subscribed, and won't keep trying to email an address MailChimp already flagged as bad.

### Message Center Alerts Not Clearing Automatically
Fixed an issue where old Message Center alerts weren't getting marked as read automatically like they should.
