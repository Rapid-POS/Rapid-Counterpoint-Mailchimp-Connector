# RapidPOS Connector for Mailchimp v3.00.00 Release Notes

**Release Date:** July 29, 2026

_Adds two-way syncing with Mailchimp, text message (SMS) customer support, and an on-demand sync option, plus several reliability fixes._

## New Features & Improvements

### Additional Mailchimp download configuration
Down-sync (Mailchimp → Counterpoint) now has more configuration options.

* Field mapping for the download direction is now configurable, separately for new customers vs. updates — matching the flexibility already available for uploads.
* Downloading from Mailchimp and uploading to Mailchimp now run on separate schedules. Uploads stay frequent (every minute by default); downloads can now run less often (a few times a day by default).

### Text message (SMS) support
You can now enroll and sync customers who only have a phone number, no email address.

* Two new enrollment options: one requires a phone number and no email, the other accepts either an email or a phone number.
* A text-only contact created or changed directly in Mailchimp is now brought down into Counterpoint the same way an email contact is.
* A customer's texting consent is now synced in both directions between Mailchimp and Counterpoint, the same as email marketing consent already was.
* Administrators can now configure how Mailchimp's subscribed/unsubscribed status gets translated into your own Counterpoint fields, for both email and text opt-in.
* Requires an active Mailchimp SMS Marketing subscription — SMS is an add-on to Mailchimp's paid plans, requires a separate application and approval, and is billed through monthly SMS credits.

### Run a sync on demand
A new option lets you trigger an out-of-cycle sync immediately instead of waiting for the next scheduled run — available from a command-line option and from a new toolbar button on the Counterpoint Customers screen.

### More consistent duplicate handling
When two Counterpoint customers share an email address or phone number, the connector now consistently picks the customer with the most recent sale to update.

## Bug Fixes

### Customer order totals now sync to Mailchimp
A bug prevented each customer's lifetime spend and order count from ever being pushed to Mailchimp, even when this option was enabled. This is fixed.

### Items no longer get stuck mid-sync
If an item sync was interrupted by a service restart or crash, affected items could get stuck and never retried. Items are now tracked in batches so an interrupted sync is automatically picked back up on the next run.

### Unsubscribes no longer get silently undone
A customer who unsubscribed from marketing emails in Mailchimp could get flipped back to subscribed on a later sync. This no longer happens.

### Fixed customers no longer get stuck
A customer whose email address was corrected after being flagged as invalid could get stuck and never resume syncing. Corrected customers now resume syncing normally.

### Oversized values from Mailchimp no longer break a sync
A value coming from Mailchimp that's longer than its destination field in Counterpoint is now automatically shortened to fit, instead of causing that customer's sync to fail.

### Sync rules now update correctly for every account
On a system with more than one configured Mailchimp account, only one account's sync rules were being refreshed when settings changed, leaving the others running on outdated rules. This now applies correctly to every account.

### Tag rules no longer block a customer's sync
An account with auto-enroll and at least one tag rule configured could hit an error that stopped that customer's entire sync from completing. Fixed.

### Corrected a timing mismatch
A timezone mismatch in the internal check for which system had the more recent change could cause updates to be applied out of order. This is corrected.

## Maintenance

* General performance and reliability improvements to the core customer-sync and item-sync engines, plus expanded internal test coverage and internal cleanup.
