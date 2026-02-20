# Rapid POS Mailchimp Connector v2.02.05 Release Notes  
Release Date: February 23rd, 2026  

---

## New Functionality

### Mailchimp Customer Tag Syncing

Added support for automated Mailchimp customer tags based on customer data in Counterpoint.  

Tags are defined using custom SQL queries such as loyalty points or total customer spending.  

Synced tags can be used in Mailchimp for campaign targeting, segmentation, and automated journeys.  

Notes:  

The connector can add tags to Mailchimp but it cannot remove them.  

Tag removal can be handled using Mailchimp Automations (flows).  

Tag configuration requires setup assistance from Rapid – billed at T&M rates.  

For a detailed guide on Mailchimp Customer Tags as they relate to the Counterpoint to Mailchimp connector, please visit:  
https://github.com/Rapid-POS/Rapid-Counterpoint-Mailchimp-Connector/blob/main/README.md#section-4-mailchimp-customer-tags  

---

## Bug Fixes and Performance Enhancements

### Updated Install Script for MailChimp Customer's Up Field Mapping Table

Adjusted the install script for the `USER_MAILCHIMP_CUSTOM_FIELD_MAPPING` table.  

Added `ADDRESS` as one of the default fields in the custom field mapping.  

---

### Mailchimp Customer Status View

The Mailchimp Customer Status View displays the synchronization status of customer records:

| Status | Description   | Count |
|--------|--------------|--------|
| 0      | Synched      | 630    |
| 1      | Pending Sync | 23     |
| 5      | Invalid Email| 120    |
| 9      | Error syncing| 3      |

---

### Updated Mailchimp Customer Status View

Displays a summary table showing:

- Each sync status code (`0`, `1`, `5`, `9`)  
- The total number of customer records currently associated with each status  

Notes:

- If no customer records exist for a given status, that status will not appear in the table  
- The table can be refreshed at any time to display the most up-to-date information  
- Best viewed in table view  
