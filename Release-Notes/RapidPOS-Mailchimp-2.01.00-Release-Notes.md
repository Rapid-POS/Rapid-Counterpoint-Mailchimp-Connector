
# Rapid POS Mailchimp Connector v2.01.00 Release Notes  

_Release Date: June 2024_

---

## New Functionality

### Error Statuses
- Set Mailchimp status to **5** for invalid email and **9** for any other error.  
- These statuses help Counterpoint users filter customers who are not syncing successfully.  
- If the customer is in a status of **5**, they have an invalid email address in Counterpoint.  
- If the customer is in a status of **9**, Mailchimp returned an error for that customer which should be investigated.  

### Custom Field Mapping
- Enabled mapping for static fields such as **Zip Code** and **Customer Number**.  
- These fields can now be synced to Mailchimp from Counterpoint using merge fields with a column type of **D** for Default.  

### Error Messages
- Added new configuration option: **Mark Alerts as Read in Message Center After Days** (default: 3 days).  
- Added a new menu item: **Mark All Mailchimp Messages as Read**.  
- Purged all Mailchimp alerts from the Message Center during upgrades.  
- Added more user-friendly error message alerts, especially for 404 errors.  

### Skip Merge Validation
- Added support for Mailchimp’s new API endpoint to **bypass required field validation** on customer uploads.  
- When enabled, customers can be pushed to Mailchimp even if required Mailchimp fields (like phone number) are blank in Counterpoint.  
- Typically, required fields in Mailchimp are used only when collecting customer information via a website sign-up form.  

### Retain Counterpoint Value
- Added an option to **retain Counterpoint values** when a Mailchimp field is empty.  
- Prevents overwriting existing data in Counterpoint with blanks when importing customers from Mailchimp.  

### Connector Version Visibility
- The Mailchimp Connector version number is now visible in the configuration.  

### Workgroup and Customer Template
- Added a **workgroup (230)** and customer template for `CRM_MLCHMP`.  
- By default, the CRM_MLCHMP template customer includes First and Last names set to **MISSING**.  
  - This ensures that if those fields are blank on import, the connector still creates the customer record with the email address.  
  - These placeholder values can later be filtered and corrected in Counterpoint.  
- When **Skip Merge Validation** is not used, these pre-defined values can also be sent to Mailchimp from the customer template for required text fields.  

### Member Sync Lookback
- Added functionality to retrieve all members who have changed in Mailchimp since the last sync date minus three days, improving sync efficiency.  

---

## Bug Fixes and Performance Enhancements
- **Business Names:** Prevented overwriting company names in Counterpoint with first and last names during import.  
- **Date Comparison:** Fixed a date comparison bug.  
- **Data Dictionary:** Removed unused data dictionary definitions.  
