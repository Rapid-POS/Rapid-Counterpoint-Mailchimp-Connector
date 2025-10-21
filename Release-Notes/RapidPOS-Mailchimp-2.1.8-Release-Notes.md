# Rapid POS Mailchimp Connector v2.1.8 Release Notes

_Release Date: February 2025_

---

## New Functionality

### CI/CD – Continuous Integration/Continuous Deployment
- Introduced CI/CD improvements to streamline deployment and version management for the Mailchimp Connector.  

### Email Address Update Logic
- Updated logic for handling customer email address changes in Counterpoint.  
- Previously, a new Mailchimp profile was created when an email address changed.  
- The new logic now updates the **existing Mailchimp profile** using the **Mailchimp ID**.  
- If the newly updated email address from Counterpoint already exists and is attached to a different profile in Mailchimp:  
  - An error will be written to the **Message Center**, and  
  - The email address for that Mailchimp profile will **not** be updated.  

---

## Bug Fixes and Performance Enhancements

### CRM_MLCHMP Customer Template Field Values
- Fixed logic to properly respect `CRM_MLCHMP` customer template field values when importing customers from Mailchimp into Counterpoint.  
- Adjusted logic so that **populated values** on the CRM_MLCHMP template are respected, while **blank or empty values** are ignored during import.  

### SQL Install Script
- Updated the SQL install script to populate empty configuration values with defaults from CI/CD.  
- If no CI/CD default exists, the configuration value remains empty.  

### Import Customer Functionality
- Modified import functionality so that **only subscribed customers** in Mailchimp are imported into Counterpoint.  
- **Unsubscribed customers** will no longer be imported.  

### Friendly Error Message for Character Length Violation
- Added a user-friendly error message when imported Mailchimp data exceeds character limits in Counterpoint.  
- The connector will throw an error and indicate that the length should be reduced in Mailchimp before re-import.  

### Loyalty Card Number (LOY_CARD_NO)
- Adjusted the SQL install script to **stop automatically setting** a value in the `CRM_MLCHMP` customer template.  
- This change accommodates clients who automatically calculate loyalty card numbers (e.g., using `PHONE_1` as the loyalty card number).  

---

## Schema Change

### Mailchimp Merge Field Mapping Table
- The Mailchimp connector includes default merge fields as part of the **Mailchimp Merge Field Mapping** view table (`USER_VI_MAILCHIMP_MERGE_FIELDS`).  
- Changes were made to ensure that updates to the `USER_MAILCHIMP_CUSTOM_FIELD_MAPPING` table are automatically reflected in this view when database schema changes occur.  
- The custom field mapping table now includes an **insert/update trigger** that automatically updates the view when a schema change occurs (such as adding a new column).  
