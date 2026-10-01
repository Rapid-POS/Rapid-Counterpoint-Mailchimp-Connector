# MailChimp v3.01.00 Release Notes

**Release Date:** October 4, 2026

This release fixes how MailChimp subscription changes come back into Counterpoint and adds Unsubscribed and Archived sync statuses.

## New Features & Improvements

### Unsubscribed and Archived sync statuses

A customer's MailChimp sync status can now show Unsubscribed, and Archived is easier to tell apart.

* A customer shows Unsubscribed when they've opted out on the channels your account enrolls on: email for Email Only, text for SMS Only, both for Both, and every channel they have for Email or SMS.
* Editing an Unsubscribed customer in Counterpoint no longer puts them back in Pending Sync. They only change back when a download finds them subscribed again.
* Customers who were already Archived stay Archived. The update moved them over for you.

### Unrelated edits no longer block newer MailChimp changes

If you change something like an address or phone number in Counterpoint, a newer subscribe or unsubscribe from MailChimp still updates the customer.

* If the opt-in itself was changed in Counterpoint more recently than in MailChimp, the Counterpoint choice wins and is sent up to MailChimp.

### Default conversion for MailChimp email status

If your account doesn't have its own setting, MailChimp's email status now converts into the Opt-In field on download.

* Subscribed sets the customer as opted in. Unsubscribed or Cleaned sets them as opted out.
* Any other status, such as Pending, leaves the Opt-In field alone.

### Sales activity is picked up when the drawer is posted

Items and tickets sold to MailChimp customers, along with each customer's running order totals, are now registered at drawer post instead of as each ticket is committed.

### Logging and database consistency

* The download log now shows the phone number and customer number for each text contact.
* Opt-in conversion rules behave the same on every database, case sensitive or not.

## Bug Fixes

### SMS unsubscribes made in MailChimp now reach Counterpoint

Unsubscribes and STOP replies in MailChimp now update the customer's text opt-in in Counterpoint.

* SMS Opt In is set to match, and the sync status changes to Unsubscribed where it applies.
* Each download checks all of your text contacts, not just the ones MailChimp reports as recently changed.
* Customers who subscribe again in MailChimp are picked up the same way.
* Opting a contact back in from Counterpoint won't re-subscribe them. They have to subscribe again in MailChimp, and the next download will pick it up.

### Opt-in boxes no longer checked for customers without an email or phone

Customers with no email address no longer show the email Opt-In checked, and customers with no phone number no longer show SMS opt-in set.

* When you add the email or phone number later, the opt-in is set correctly at that point.
* The update cleaned up existing customers that were in this state.

### Customers who opted out by text are no longer re-sent a subscription

We were sending new text subscriptions to customers MailChimp already had as opted out. MailChimp rejected them and the customer got stuck in an error state.

* Their SMS Opt In in Counterpoint is now set to No instead.
* Their name and other details still sync.

### Text-only customers with an incomplete address now sync

Customers with no email and an empty or partial address were failing to sync to MailChimp. They sync now.

* The address is only sent when street, city, state and zip are all filled in.
* If MailChimp still rejects a detail, we leave it out, log it, and sync the rest of the customer.

### Last sync date now saves

The last sync date on the MailChimp setup now saves after a download, and it only moves forward when the download finishes. A failed download won't skip the changes it missed.

* The Archived contacts check works the same way.
