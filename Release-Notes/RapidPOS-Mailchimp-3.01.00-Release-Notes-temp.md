## Mailchimp Sync Status Codes

Each Mailchimp customer record includes a sync status value indicating its current state in the sync process:

| Status | Description |
|--------|-------------|
| 0 | Fully synced; nothing pending |
| 1 | Recently created or updated; will sync on the next connector run |
| 2 | Profile is currently in the active sync queue |
| 3 | Unsubscribed in Mailchimp. Which channels count depends on the Auto Create Mailchimp Profile setting; see [Which Statuses Count as Unsubscribed](#which-statuses-count-as-unsubscribed) |
| 4 | Archived in Mailchimp (set by the daily Archived Lookup job) |
| 5 | Invalid email address, or the customer has neither an email address nor a phone number |
| 6 | Not set by the connector. An invalid phone number is flagged with the **Invalid SMS Phone Number** checkbox instead |
| 7 | Reserved; not currently used |
| 9 | Sync error; requires remediation before it can be re-synced |

> **Upgrade note:** Archived was previously status 10. The install script moves existing rows from 10 to 4.
