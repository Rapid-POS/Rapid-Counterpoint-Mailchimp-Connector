# Rapid Mailchimp Connector  
### Client Information for version 2.1.9  
**Updated April 18, 2025**

---

## Table of Contents
- [Overview](#overview)
- [Customer Information](#customer-information)
  - [Addresses](#addresses)
  - [Birthdays](#birthdays)
  - [Calculated Fields](#calculated-fields)
- [Ticket & Item Information](#ticket--item-information)
- [Syncing Audiences and Contacts](#syncing-audiences-and-contacts)
  - [Audience](#audience)
  - [Contacts/Customers](#contactscustomers)
- [Connector Sync Process](#connector-sync-process)
- [Common Customer Sync Questions](#common-customer-sync-questions)
  - [Multiple Customers with the Same Email Address](#multiple-customers-with-the-same-email-address)
  - [Merging Customer Records in Counterpoint](#merging-customer-records-in-counterpoint)
  - [Updating an Existing Customer’s Email Address](#updating-an-existing-customers-email-address)
  - [Sync Errors](#sync-errors)
- [Importing Customers from Mailchimp into Counterpoint](#importing-customers-from-mailchimp-into-counterpoint)
  - [Limitations](#limitations)
  - [Caution Regarding Creating Duplicate Customers in Counterpoint](#caution-regarding-creating-duplicate-customers-in-counterpoint)
  - [Determining which Fields to Import](#determining-which-fields-to-import)
  - [Using the Template Customer](#using-the-template-customer)
  - [Using Skip Merge Validation](#using-skip-merge-validation)
  - [First and Last Name versus Business/Company Name](#first-and-last-name-versus-businesscompany-name)
- [Additional Mailchimp Tools](#additional-mailchimp-tools)
  - [Mailchimp Configuration](#mailchimp-configuration)
  - [Mailchimp Field Mapping](#mailchimp-field-mapping)
  - [Run Mailchimp Connector](#run-mailchimp-connector)
  - [Mark All Mailchimp Messages as Read](#mark-all-mailchimp-messages-as-read)

---

## Overview

The Mailchimp connector pushes customers up from Counterpoint to Mailchimp. If desired, it can also pull customers down into Counterpoint. In addition, it can send some limited sales history data. This allows users to create segments, for example, customers who have purchased an item in a certain category.

---

## Customer Information

It is important to distinguish between information about a customer as opposed to information about a ticket or an item. Mailchimp accepts a wide range of information about customers; however, it accepts very limited information about tickets and items.

Here are some examples of information that can be sent about customers:

- **`Email 1`**  
- **`Customer Number`** *  
- **`First Name`**  
- **`Last Name`**  
- **`Address + City + State + Zip`**  
- **`Zip Code`** *  
- **`Phone 1 (Home Phone)`**  
- **`Customer Category`**  
- **`First sale date`**  
- **`Last sale date`**  
- **`Loyalty point balance`**  
- **`A/R account balance`**

\* Must be sent as merge field column type = default.

The Mailchimp connector uses custom field mapping so nearly any field from the Counterpoint customer record can be sent to Mailchimp. Here are a few things to keep in mind:

---

### Addresses

Mailchimp requires that the full address of the customer be combined into one field with a column type of address. Therefore, **`Address 1`**, **`Address 2`**, **`Address 3`**, **`City`**, **`State`**, and **`Zip Code`** are combined and sent to Mailchimp as a singular field.

If you would also like these fields sent individually, such as **`Zip Code`**, these can also be configured as separate custom fields to allow for segment creation.

If you want to import customer address information, be sure that the merge field column type is set to **address**. Then Mailchimp will create a form where each element of the address is separated so that it can be imported properly into Counterpoint.

---

### Birthdays

If you store birthday information in Counterpoint, this can be sent to Mailchimp as a custom field with a merge field column type set to **birthday**. However, Mailchimp will not accept a birthdate with a year in it—it only accepts Month and Day. Therefore, the birthday field will be sent to Mailchimp with the year removed.

If you are configured to import customers from Mailchimp, the birthday will be imported into Counterpoint as `MM/DD/1900`. The year of 1900 will be added since Mailchimp will not send a year, but Counterpoint requires one for date fields.

---

### Calculated Fields

Sometimes calculated fields can also be sent to Mailchimp. These requests are reviewed and quoted individually by our programmers.

**Example of a calculated field:**  
The date a customer last purchased a product in a certain category.

---

## Ticket & Item Information

Mailchimp accepts a limited amount of ticket information. This information is pushed to the “Ecommerce” fields in Mailchimp since Mailchimp does not provide separate fields for POS data.

### Ticket Header/Overall
- **`Order Total`**  
- **`Tax`**

### Ticket Lines
- **`Item Number`**  
- **`Item Description (Product Title)`**  
- **`Quantity Purchased`**  
- **`Price`**

### Items
- **`Item Number`**  
- **`Item Description (Product Title)`**  
- **`Item Vendor (category/subcategory/vendor)`**

### Additional Customer Data
- **`Total number of tickets for that customer`**  
- **`Total spent`**

While Mailchimp does offer custom merge field mapping for customer information, Mailchimp does **NOT** offer custom field mapping for products. Only the values above can be sent.

Only posted tickets are sent to Mailchimp. When a drawer is posted, the new tickets are then pushed to Mailchimp.

If desired, during the install of the connector, previous sales history can be configured to send on the first sync. For example, you can choose to send the previous **60 days**, **365 days**, **525 days**, etc.

For the **`Item/Product Vendor`** field, Rapid has added functionality to send up to three pieces of information in this single field:
- Category  
- Subcategory  
- Vendor  

**Example:**  
If you have a category for “coffee,” a Mailchimp segment could be created by setting the filter to:  
> Vendor purchased contains coffee.

---

## Syncing Audiences and Contacts

In Mailchimp, an **Audience** (sometimes called a list) will have **Contacts** (sometimes called customers). Emails can be sent to an entire audience or sometimes just a segment of the audience.

---

### Audience

If a client already has an audience set up in Mailchimp, they can choose to use that audience for their Counterpoint connection. If they do not yet have an audience, the Mailchimp connector will create one titled **Counterpoint**. The audience defined in the Mailchimp configuration is the only one that will receive information from Counterpoint.

---

### Contacts/Customers

The connector only pushes customers up who have an email address in **`Email 1`**.

- If the **`Opt-Out of Email Marketing`** checkbox is unchecked, the customer will be pushed up as an active contact.  
- If the **`Opt-Out of Email Marketing`** checkbox is checked, the customer will be pushed up to Mailchimp as an opted-out customer meaning that email address will be designated as unsubscribed in all of Mailchimp.

---

## Connector Sync Process

The connector runs every **15 minutes**.

1. The connector gets every customer who is unsubscribed in Mailchimp and sets the opt-out flag in Counterpoint.  
2. It then checks customers whose subscription status has changed in Mailchimp and compares timestamps with Counterpoint.  
3. If the customer was opted-out in Counterpoint, unchecking the opt-out flag will re-add them to Mailchimp—unless they unsubscribed directly through a Mailchimp link, in which case Mailchimp will not allow resubscribe via Counterpoint.  
4. Lastly, any customers that have changed since the last run, plus new customers, are synced up to Mailchimp.  
5. If enabled, customers changed in Mailchimp (via website or signup form) will sync down into Counterpoint.

---

## Common Customer Sync Questions

### Multiple Customers with the Same Email Address

- If one of the customers has already synced (has a **Mailchimp ID**), that one takes precedence.  
- If none have synced, the customer with the most recent **`Last Sale Date`** is synced.  

---

### Merging Customer Records in Counterpoint

If both customers have already been synced to Mailchimp in the past, then they both have a **Mailchimp ID**. The Mailchimp ID on the “To” customer will be retained when merging, and the “From” customer’s Mailchimp ID will not be retained.

---

### Updating an Existing Customer’s Email Address

If the customer has already been synced, the **Mailchimp ID** is retained, and their email address is updated in Mailchimp.

However, if the new email address already exists on a different profile in Mailchimp, an error will appear in the message center and the email will not be updated.

---

### Sync Errors

If a customer is not syncing, check their **`Sync Status`**:

| Sync Status | Meaning |
|--------------|----------|
| 0 | Customer has been synced to Mailchimp |
| 1 | Customer will be added to the sync queue on the next run |
| 2 | Customer is in the active sync queue |
| 5 | Customer has an invalid email address as determined by Mailchimp |
| 9 | Customer encountered an error when syncing and should be investigated |

To view a customer’s sync status, open the customer lookup screen and add a column for **`Mailchimp Stat`**.

[Image Placeholder]

You can also filter the customer lookup to display customers with a specific Mailchimp stat for remediation.

[Image Placeholder]

---

## Importing Customers from Mailchimp into Counterpoint

If desired, customers can be imported from Mailchimp into Counterpoint. This can be useful if you want customers who sign up on your website to automatically have a customer record created in Counterpoint.

---

### Limitations

The connector will **ONLY** import customers who are part of the configured audience.

---

### Caution Regarding Creating Duplicate Customers in Counterpoint

If you did not previously capture email addresses in Counterpoint, the connector cannot match existing records with new ones from Mailchimp, creating duplicates.  
These can be merged manually using Counterpoint’s **Merge Customer Utility**.  
Consult with your **BA**, **Care Team**, or **Project Manager** before enabling this—especially if you use **Driver License Scan**.

---

### Determining which Fields to Import

Carefully consider which fields you want to import from Mailchimp into Counterpoint.  
These can be configured individually.

Always set **`Retain Counterpoint Value if Mailchimp is Empty`** on these fields.  
This prevents overwriting existing data in Counterpoint with blanks from Mailchimp.

---

### Using the Template Customer

By default, the **`CRM_MLCHMP workgroup 230 Template`** customer has **First** and **Last names** as `*MISSING*`.  
If these values are not populated, at least the email will import, and the missing values can be corrected later.

When not using **Skip Merge Validation**, these predefined values can also be sent to Mailchimp to populate required fields.

---

### Using Skip Merge Validation

For clients with website signup forms requiring certain fields, enabling **Skip Merge Validation** forces the sync to proceed even when required fields are missing.

---

### First and Last Name versus Business/Company Name

When importing data, if the existing Counterpoint customer is marked as a **business**, this will **not** be overwritten by **First/Last Name** data from Mailchimp.

---

## Additional Mailchimp Tools

### Mailchimp Configuration

Clients can view their configuration anytime by navigating to:  
**Connectors > Mailchimp > Mailchimp Configuration**  
To make changes, please consult with Rapid.

---

### Mailchimp Field Mapping

Each customer merge field can be viewed under:  
**Connectors > Mailchimp > Mailchimp Field Mapping**  
To adjust mapping, consult with Rapid.

---

### Run Mailchimp Connector

The connector runs automatically every **15 minutes** but can also be run manually by clicking **Run Mailchimp Connector** in Counterpoint.  
A black box will appear during execution—do not close it.

---

### Mark All Mailchimp Messages as Read

If users encounter frequent pop-up alerts about Mailchimp issues, they can use **Mark All Mailchimp Messages as Read** to stop alerts and review them later.

---

[End of Document]
