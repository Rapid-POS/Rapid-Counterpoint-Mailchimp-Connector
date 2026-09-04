# RapidPOS Mailchimp Connector v3.00.08 Release Notes
**Release Date:** September 06, 2026

Fixes two data-sync issues affecting register transactions and customer syncing.

## Bug Fixes

### Fixed NULL error blocking transactions with Customer/Rewards accounts
A disabled database trigger left over from a prior update wasn't fully removed, causing registers to throw a SQL error and block the transaction whenever a Customer/Rewards account was selected or Rewards were redeemed.

* Removed the leftover trigger so Customer/Rewards transactions process normally.
* Updated ticket-history sync validation so it no longer requires a customer to already have a Mailchimp profile before posting to the loyalty program.

### Fixed manual sync button not pushing customer updates
The manual run button wasn't functioning correctly, so new customers and opt-out status changes made in Counterpoint weren't being pushed to Mailchimp.

* New customers and subscription status changes now sync to Mailchimp as expected when the manual run is triggered.
