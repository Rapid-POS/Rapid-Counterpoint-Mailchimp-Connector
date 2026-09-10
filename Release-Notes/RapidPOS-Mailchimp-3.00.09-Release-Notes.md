# RapidPOS Connector for MailChimp v3.00.09 Release Notes - Coming Soon
**Release Date:** September 13, 2026

Fixes for item categorization, discontinued product syncing, and customer data handling.

## Bug Fixes

### Product Category and Subclass Data Now Syncs Correctly
Items were syncing to Mailchimp without their subclass data, so subclass-based customer segments returned far fewer contacts than they should have.

* Item sync now includes subclass data in every product record sent to Mailchimp.
* Fixed a related issue where items with size or variant tracking were treated as if they had none.
* Products that already synced won't pick up the corrected category data on their own — they'll need to be resent to Mailchimp.

### Discontinued Items Can Be Corrected After Syncing
Once an item was marked discontinued in Counterpoint, its Mailchimp product was locked and couldn't be updated again, even if it had synced with incomplete or incorrect data beforehand.

* An item that already synced at least once stays eligible for correction after being discontinued.
* Items that never synced still won't create a new Mailchimp product once discontinued — that part hasn't changed.

### Sync Errors No Longer Affect Other Records in the Same Batch
Certain items and customers were causing sync errors that stopped other records in the same batch from processing.

* Items missing vendor, category, or subcategory data now sync with a blank value instead of failing.
* Customers with no last name on file no longer cause the customer sync to fail.
* One bad customer record can no longer block the rest of the batch.

### Manual Run Button Fixed
The manual sync run button wasn't working correctly. It's fixed.
