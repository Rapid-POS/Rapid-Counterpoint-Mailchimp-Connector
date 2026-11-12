# MailChimp v3.01.00 Release Notes - COMING SOON

**Release Date:** TBD

_Customer syncing to MailChimp is now paced after each update, and the connector stops adding new customers before your account goes over its MailChimp plan's contact limit. You can limit orders to chosen stores, archived contacts are detected automatically, and we fixed many opt-in and sync problems._

## Action Required After This Update and All Future Updates

Starting with this release, the MailChimp connector slows down customer syncing after every update. This is called throttling. While it is on, the connector sends only a small batch of customers to MailChimp each day (by default, 3.33% of your plan's contact limit). Items, orders and downloads from MailChimp are not affected.

Throttling gives you a chance to confirm that customers are coming through correctly after an update before the rest of your customer list follows. If something looks wrong, only a small share of your customers will have been sent.

Throttling stays on until you turn it off, so after each update:

1. Wait for the first batch of customers to reach MailChimp. A Message Center alert tells you throttling is on, and a daily reminder shows how many customers are still waiting.
2. In MailChimp, look over a few of the recently synced contacts. Check that their names, email addresses, phone numbers and subscribe status match what you have in Counterpoint.
3. If everything looks right, choose Connectors > MailChimp > Disable MailChimp Throttle in Counterpoint. The rest of your customers start syncing right away, with no restart needed.

If something does not look right, leave throttling on and contact RapidPOS support.

## New Features & Improvements

### Customers are sent at a reduced pace after an update

After an update, the connector now sends customers to MailChimp at a reduced daily pace until you turn throttling off. See Action Required above for the steps.

- The connector turns throttling on for every account the first time this update runs, and again after any later version change.
- You can change the daily percentage in "Customer throttle (% of plan contact limit per day)". Customers over the daily limit wait until the next day.
- Turning throttling off takes effect right away with no restart.
- The Configuration screen shows whether throttling is on and the daily percentage, for viewing only.

### MailChimp contact limit protection

The connector now stops adding new customers to MailChimp before your account goes over its plan's contact limit.

Every MailChimp plan comes with a contact allowance, for example 2,500 contacts, and MailChimp bills on the highest number of contacts your account ever reaches. Before this update, the connector could keep sending Counterpoint customers to MailChimp past that number, which could move your account into a more expensive tier or trigger overage charges.

- Your plan's contact allowance is shown in "MailChimp plan contact limit" on the Configuration screen.
- The connector compares this limit with "Current MailChimp contact count", which shows the latest count read from MailChimp.
- "Allowable overage (contacts over the plan limit)" lets you accept some contacts over the limit, or stop a set number of contacts below it.
- You get a Message Center alert at 90% and 98% of the limit, so you have warning before anything is held. You also get an alert when new customers are being held.
- At the limit, customers already in MailChimp keep updating while new customers wait. To resume, upgrade your MailChimp plan and raise the contact limit.

### Send orders only for the stores you choose

_Applies only if your connector is set to send sales to MailChimp._

A new setting lets you choose which Counterpoint stores send their orders to MailChimp. Use it to avoid sending orders twice when some of them already reach MailChimp another way, such as from Shopify.

- By default the list is blank, and the connector keeps sending orders from every store, as it did before this update.
- To limit which stores send orders, enter their store IDs, separated by commas, in "Stores to send orders for", in the Sales section of the Configuration screen.
- The connector does not pick up sales from stores that are not on the list. Changing the list does not take back orders that were already sent.

### Archived contacts are detected automatically

Customers whose contact has been archived in MailChimp now show as Archived in Counterpoint without anyone having to mark them.

- A new daily check runs at 2 AM. You can change the time in "Archived Lookup Execution Time".
- The regular download and the text-only customer sync also flag an archived contact as soon as they see it, rather than waiting for the daily check.
- When MailChimp reports a customer's email as permanently deleted, the connector now marks the customer Archived instead of failing and retrying every cycle. That contact has to subscribe again in MailChimp.

### Unsubscribed and Archived sync statuses

We added an Unsubscribed status and a clearer Archived status to each customer's MailChimp sync status.

- A customer shows Unsubscribed when they have opted out on the channels your account enrolls on: email for Email Only, text for SMS Only, both for Both, and every channel they have for Email or SMS.
- Editing an Unsubscribed customer in Counterpoint no longer sends them back to Pending Sync. They only change back when a download finds them subscribed again.
- Customers who were already marked Archived stay Archived. The update moves them over automatically.

### Newer MailChimp changes are no longer blocked by unrelated edits

An unrelated change to a customer in Counterpoint, such as a new address or phone number, no longer stops a newer MailChimp subscribe or unsubscribe from updating that customer.

- If the opt-in itself was changed in Counterpoint more recently than in MailChimp, the connector keeps the Counterpoint choice and sends it up to MailChimp.
- When the download skips a customer, the log now shows the dates it compared.

### Your customer filter and opt-in settings now apply when customers are enrolled automatically

The connector now enrolls customers automatically only when they match your customer filter and, if you have set an opt-in expression, are opted in on the channels your account enrolls on.

- A customer who is already enrolled but stops matching the customer filter is taken off the sync queue.
- Archived customers are left alone.

### Customers who already have a MailChimp ID are carried over on a new setup

When you set up a new MailChimp account, the connector enrolls customers who already have a MailChimp ID with their existing status, so it does not send already-synced customers to MailChimp again. For an account that is already installed, RapidPOS support can run a one-time carry-over. It does not run automatically, so an account that enrolls only some customers is never swept in.

### Orders wait for their items instead of raising errors

_Applies only if your connector is set to send sales to MailChimp._

MailChimp only accepts an order when every item on it already exists in your MailChimp store. Before this update, if a customer bought an item the connector had not sent to MailChimp yet (for example, a new item sold the same day it was added), the order was rejected and logged as an error.

- The connector now spots the missing item, queues it to be sent, and holds the order instead of sending it early.
- Once the item is in MailChimp, the order goes out automatically. You do not need to do anything.
- If MailChimp cannot accept the item itself, for example because of a problem with the item's details, its orders keep waiting. The error is logged for the item rather than for each order, so you know what to fix. Once the item is sent successfully, the held orders go out.

### Read MailChimp alerts are cleaned out of the Message Center

The connector now deletes MailChimp alerts from the Message Center 30 days after every recipient has read them.

- Unread alerts are never deleted.
- Change the number of days in "Delete Read Alerts from Message Center After Days (0 = never)". Enter 0 to turn the clean-up off. Restart the connector service after changing it.

### Default conversion for MailChimp email status

Accounts without their own setting now convert MailChimp's email status into the Opt-In field automatically on download.

- Subscribed sets the customer as opted in. Unsubscribed or Cleaned sets them as opted out.
- Any other status, such as Pending, leaves the customer's Opt-In field as it was.

### Items, tickets and order totals are picked up when the drawer is posted

_Applies only if your connector is set to send sales to MailChimp._

The connector now registers items and tickets sold to MailChimp customers, along with each customer's running order totals, when the drawer is posted instead of as each ticket is committed.

### Items are skipped when sending sales is turned off

_Applies only if your connector is set to send sales to MailChimp._

When sending sales is turned off, the connector no longer sends items to MailChimp either, and it writes nothing about items or sales to the log. Items pick up where they left off when you turn sales back on.

## Bug Fixes

### Customers already in MailChimp are no longer set back to Subscribed

A customer who had already unsubscribed in MailChimp was set back to Subscribed when the customer sync sent them up with no opt-in choice recorded in Counterpoint.

- For a customer who already exists, the connector now sends back MailChimp's own current status, so nothing changes.
- A brand-new customer is still sent as Subscribed.

### SMS unsubscribes made in MailChimp now reach Counterpoint

A text subscription changed in MailChimp, such as an unsubscribe or a STOP reply, now updates the customer in Counterpoint.

- The customer's SMS Opt In is set to match, and their sync status updates to Unsubscribed where it applies.
- Each download now checks all of your text contacts, not only the ones MailChimp reports as recently changed.
- A customer who subscribes again in MailChimp is picked up the same way.

### Text subscriptions are no longer rejected by single opt-in audiences

MailChimp audiences set up for single opt-in were rejecting customers opted in to text. A new "SMS Double Opt-In" setting now matches your MailChimp audience. It defaults to No, which suits single opt-in audiences.

### Customers who opted out by text are no longer re-sent a subscription

The connector no longer sends a new text subscription for customers MailChimp already holds as opted out by text. MailChimp rejected these, which used to leave the customer stuck in an error state.

- Their SMS Opt In in Counterpoint is set to No instead.
- Their name and other details still sync.

### Item numbers containing a slash now sync

_Applies only if your connector is set to send sales to MailChimp._

Items with a slash in the item number, such as AB/100, could not be updated in MailChimp, and their orders could not be matched to them.

- These items now go to MailChimp under a slash-safe ID, and orders use the same ID.
- The connector cannot edit products already created in MailChimp with a raw slash. Remove them in MailChimp and the connector creates the item again on its next sync.

### Items MailChimp says already exist are no longer retried every cycle

_Applies only if your connector is set to send sales to MailChimp._

An item that MailChimp reported as already existing, even though the connector could not find it, failed the same way on every cycle.

- The connector now records the item as in MailChimp and sends its latest details where possible, so orders can use it.
- It sends the item again only when it is edited in Counterpoint.

### One bad text contact no longer stops the whole download

A single text contact with unexpected data no longer ends the download and skips every contact after it. The connector logs the problem contact and carries on with the rest.

### Customers without an email or phone no longer show opt-in boxes checked

Customers with no email address no longer show the email Opt-In checked, and customers with no phone number no longer show the SMS opt-in set.

- If you add the email address or phone number later, the opt-in is worked out correctly at that point.
- The update cleaned up existing customers in this state.

### Text-only customers with an incomplete address now sync

Customers with no email whose address was empty or only partly filled in no longer fail to sync to MailChimp.

- The connector sends the address only when the street, city, state and zip are all filled in.
- If MailChimp still rejects a detail, we leave that detail out, note it in the log, and sync the rest of the customer.

### Last sync date now saves

The last sync date on the MailChimp setup now saves after a download and only moves forward when the download finished, so a failed download no longer skips the changes it missed. The same applies to the Archived contacts check.

### Orders already sent are not queued to send again

_Applies only if your connector is set to send sales to MailChimp._

A ticket posted from an order that MailChimp had already received is no longer registered a second time, which would have sent that order twice.

### Installing or updating no longer stops on older databases

The update no longer stops with an error on databases created before certain tables and columns existed.

- The update now adds missing columns on the customer tags and older field mapping tables before using them.
- We moved a test script that was running (and failing) during updates out of the update.

## Maintenance

- Lookups by customer, MailChimp ID, email, phone and sync status are faster, thanks to new indexes on the MailChimp tables only.
- The update no longer scans a store's whole ticket history. It looks only at the recent period used for order sync, and only for accounts that send sales.
- The connector now protects the stored database password with a key tied to the computer it runs on.
- The download log now shows the phone number and customer number for each text contact, so it is easier to follow a contact through a sync.
- Conversion rules for opt-in settings now work the same way on every database, whether or not it is case sensitive.

## Known Issues

### Archived is final

Once a customer is marked Archived, you cannot change it back. A contact who is restored in MailChimp stays Archived in Counterpoint.
