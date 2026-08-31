# Mailchimp Connector 3.00.07 Release Notes
**Release Date:** September 2, 2026

This release fixes duplicate loyalty reward emails, restores unsubscribe syncing between Mailchimp and Counterpoint, and resolves sync errors affecting product categories and customer records.

## Bug Fixes

### Duplicate Loyalty Reward Emails
Customers were receiving the same loyalty milestone email multiple times a day. The connector no longer re-pushes a loyalty tag to Mailchimp if that tag is already active on the member, so the reward automation only fires once per threshold reached.

* Previously, because Mailchimp clears the tag right after sending the reward email, the connector's "already tagged" check always found a clean slate on the next sync and re-sent the tag — this fix closes that gap at the source instead.

### Unsubscribes Not Syncing Back to Counterpoint
Customers who unsubscribed from marketing emails in Mailchimp kept receiving them, because unsubscribe status was never syncing back to Counterpoint. Unsubscribed, cleaned, and pending contacts are now retrieved and written back to Counterpoint correctly.

* Only opt-in/opt-out fields are updated for existing customers who are no longer subscribed — their other Counterpoint record details are left untouched.
* An unmatched contact that isn't actively subscribed in Mailchimp no longer creates a brand-new customer record in Counterpoint.

### Sync Errors Affecting Product Categories and Customer Records
A missing product vendor/category value, or a customer record with no last name on file, could crash the sync and stop category-based Mailchimp segments from updating — and one bad customer record could block every other customer queued behind it in that sync cycle.

* Product syncs now send a blank category value instead of crashing when vendor/category/subcategory data can't be resolved.
* A single customer record with sync issues (e.g., missing last name) no longer stops the rest of that batch from syncing.
