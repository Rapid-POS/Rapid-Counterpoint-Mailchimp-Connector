# Rapid POS Mailchimp Connector v2.03.00  
**Release Date:** April 15th, 2026

---

## New Functionality

### Mailchimp Item Field Mapping
Introduced the **Mailchimp Item Field Mapping module** to support dynamic assignment of product-level data within sales history.

- Enables Rapid POS Programmers the ability to map item-specific data for more detailed and flexible Mailchimp integrations
- Enhances the ability to leverage purchase history in campaigns, segmentation, and reporting

---

## Bug Fixes and Performance Enhancements

### Subscription Status Selection Fix
- Resolved an issue where the subscription status defaulted incorrectly to **Subscribed.** The update ensures accurate selection and application of the **Subscribe** status, with proper handling of both subscribed and unsubscribed states. 

### Customer Form Restrictions
- Disabled the ability to copy from the **Mailchimp ID field** within the customer form
- Prevents unintended duplication or misuse of Mailchimp identifiers
