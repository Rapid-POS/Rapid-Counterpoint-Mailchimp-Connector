# Rapid POS Mailchimp Connector v2.01.09 Release Notes

_Release Date: March 2025_

---

## Bug Fixes and Performance Enhancements

### Continue Processing Customers
- Updated **Merge Field View** to process customer records with a **sync status of 2** on the **customer table** instead of the temporary table.  
- This change ensures that customers continue to be processed correctly, even if the connector stops due to an error.  
