# Rapid POS Mailchimp Connector - Version 2.1
Updated 4/18/2025

---

## Overview

The Rapid Mailchimp Connector enables seamless data synchronization between Counterpoint and Mailchimp, helping businesses manage customer marketing lists more effectively. The connector automatically syncs customer and sales data from Counterpoint to Mailchimp to support targeted email campaigns and audience segmentation. It can also import new or updated customer information from Mailchimp back into Counterpoint, ensuring both systems stay up to date.

---

## Minimum System Requirements:
- Minimum Counterpoint version: **8.5.6.2**
- Minimum SQL Server version: **2016**

If you would like the Mailchimp connector but your system does not meet these minimum requirements, please consult your Care Team Lead (vCIO) for an upgrade quote.

---

## Table of Contents

- [Overview](#overview)
- [Minimum System Requirements](#minimum-system-requirements)
- [SECTION 1: Mailchimp Audiences and Contacts](#section-1-mailchimp-audiences-and-contacts)
- [SECTION 2: Customer Information](#section-2-customer-information)
- [SECTION 3: Ticket & Item Information](#section-3-ticket--item-information)
- [SECTION 4: Connector Sync Process](#section-4-connector-sync-process)
- [SECTION 5: Common Customer Sync Questions](#section-5-common-customer-sync-questions)
- [SECTION 6: Troubleshooting and Sync Status Codes](#section-6-troubleshooting-and-sync-status-codes)
- [SECTION 7: Importing Customers from Mailchimp into Counterpoint](#section-7-importing-customers-from-mailchimp-into-counterpoint)
- [SECTION 8: Additional Mailchimp Tools](#section-8-additional-mailchimp-tools)
- [Conclusion](#conclusion)

---

## SECTION 1: Mailchimp Audiences and Contacts

The Mailchimp Connector defines how your Counterpoint customer data interacts with Mailchimp. It ensures that your audience and contacts stay updated so that email campaigns always use the most current customer information.

### Audience

If a client already has an audience created in Mailchimp, they may choose to use that existing audience for their Counterpoint connection.  

If no audience exists, the Mailchimp Connector will automatically create one titled **Counterpoint** during setup.  

Only the audience defined in the Mailchimp configuration settings will receive information from Counterpoint.  
Data will not sync to any other audiences that may exist in the Mailchimp account.

### Contacts

The connector pushes customer records from Counterpoint to Mailchimp **only** when the customer has a valid email address in **Email 1**.

- If the **`Opt-Out of Email Marketing`** checkbox is **unchecked**, the customer is sent to Mailchimp as an **active contact** (subscribed).  
- If the **`Opt-Out of Email Marketing`** checkbox is **checked**, the customer is sent to Mailchimp as an **opted-out contact**, meaning their email address will appear as **unsubscribed** across all Mailchimp lists.

---

## SECTION 2: Customer Information

It is important to distinguish between **customer information** and information related to **tickets** or **items**.  
Mailchimp accepts a wide range of customer data but supports only limited ticket and item information.

Below are examples of customer details that can be sent to Mailchimp:

1. Email 1  
2. Customer Number *  
3. First Name  
4. Last Name  
5. Full Address (Address + City + State + Zip)  
6. Zip Code *  
7. Phone 1 (Home Phone)  
8. Customer Category  
9. First Sale Date  
10. Last Sale Date  
11. Loyalty Point Balance  
12. A/R Account Balance

\* Must be sent as **merge field column type = default.**

The Mailchimp connector uses **custom field mapping**, allowing nearly any field from the Counterpoint customer record to be sent to Mailchimp. Keep the following points in mind:

### Addresses

Mailchimp requires the **full customer address** to be combined into a single field with a column type of address.  
Accordingly, **Address 1**, **Address 2**, **Address 3**, **City**, **State**, and **Zip Code** are merged and sent to Mailchimp as one field.

If you would like to send these fields individually (for example, **Zip Code** for audience segmentation), you can configure them as separate custom fields.

When importing customer address information into Counterpoint, ensure that the merge field column type is set to **address**. Mailchimp will then generate a form where each address component is separated for proper import into Counterpoint.

### Birthdays

If birthday information is stored in Counterpoint, it can be sent to Mailchimp as a custom field with the merge field column type set to **birthday**.  

Mailchimp only accepts **month and day** — not the year — so the connector removes the year before sending the data.

When importing customers from Mailchimp, birthdays are imported into Counterpoint as MM/DD/1900.  
The year **1900** is added automatically because Mailchimp omits the year, but Counterpoint requires it for date fields.

### Calculated Fields

In some cases, **calculated fields** can also be sent to Mailchimp. These requests are reviewed and quoted individually by Rapid programmers.

**Example of a calculated field:**  
- The date a customer last purchased a product in a specific category.  

---

## SECTION 3: Ticket & Item Information

Mailchimp accepts a limited set of ticket data. This information is sent to Mailchimp’s **Ecommerce** fields, as Mailchimp does not provide dedicated fields for POS data.

### Ticket Header / Overall
- Order Total  
- Tax

### Ticket Lines
- Item Number  
- Item Description (Product Title)  
- Quantity Purchased  
- Price

### Items
- Item Number  
- Item Description (Product Title)  
- Item Vendor (Category / Subcategory / Vendor)

### Additional Customer Data
- Total Number of Tickets for that customer  
- Total Spent

While Mailchimp supports **custom merge field mapping** for customer information, it **does not** support custom mapping for products. Only the values listed above can be sent.

Only **posted tickets** are sent to Mailchimp. When a drawer is posted, the associated tickets are pushed to Mailchimp.

If desired, during connector installation, previous sales history can be included during the initial sync. For example, you can choose to send sales data from the previous 60, 365, or 525 days.

### Item / Product Vendor Field
Rapid has added functionality to include up to three pieces of information within the **Item/Product Vendor** field:
- Category  
- Subcategory  
- Vendor

**Example:**  
If you have a category for “coffee,” you can create a Mailchimp segment using the filter:  
> Vendor purchased contains coffee

---

## SECTION 4: Connector Sync Process

The Mailchimp Connector runs automatically every **15 minutes** to keep customer data synchronized between Counterpoint and Mailchimp.

### Step 1: Update Unsubscribed Contacts

The connector first retrieves every contact marked as **unsubscribed** in Mailchimp and sets the corresponding customer **opt-out flag** in Counterpoint.  

This ensures that Mailchimp unsubscribes always take precedence, maintaining compliance with customer email preferences across both systems.

### Step 2: Compare Subscription Status Changes

Next, the connector retrieves a list of contacts in Mailchimp who have changed their subscription status — either **opted-in** or **opted-out** — since the last sync.  

It then compares the **date and time** of each contact change in Mailchimp with the corresponding customer opt-in/out timestamp in Counterpoint.

- If the customer was opted-out by a user in Counterpoint, they can be re-added to Mailchimp by unchecking the **“Opt-out from marketing emails”** flag in Counterpoint.  
- If the customer unsubscribed via a Mailchimp link (for example, using the unsubscribe link in an email), Mailchimp will **not allow a resubscribe** initiated from Counterpoint.  
  In this case, the **“Opt-out from marketing emails”** flag in Counterpoint will automatically be reset to **opted-out** during the next connector run.

### Step 3: Sync Updated and New Customers to Mailchimp

After processing subscription changes, the connector identifies all customers in Counterpoint who have been **added or modified** since the previous sync.  

These records are pushed to Mailchimp, ensuring that existing Mailchimp contact information is updated with the latest data from Counterpoint.

### Step 4: (Optional) Import Updated or New Customers from Mailchimp

If enabled in your configuration, the connector can also **download customer updates** from Mailchimp into Counterpoint. This includes customers who have added or modified their information through a **website form**, **sign-up form**, or any other Mailchimp integration.

- **New contacts** in Mailchimp are automatically matched or created in Counterpoint based on the **email address**.  
- **Existing contacts** are updated in Counterpoint by matching on their **Mailchimp ID**, ensuring that recent changes made in Mailchimp are reflected in Counterpoint.

---

## SECTION 5: Common Customer Sync Questions

Occasionally, two customers in Counterpoint may share the same email address, or a customer’s email address may be updated. The following explains how the connector handles various situations.

### Multiple Customers with the Same Email Address

When multiple customers share the same email address in Counterpoint, the connector prioritizes the customer who has already been synced to Mailchimp (the one with an existing **Mailchimp ID**).  

The client will receive a message in Counterpoint indicating that the customer **without** a Mailchimp ID could not be synced due to the duplicate email address.

If none of the customers have a Mailchimp ID, the connector will sync the customer with the **most recent last sale date** and skip the other customer record(s).

### Merging Customer Records in Counterpoint

When a user determines that two customer records in Counterpoint represent the same individual or business, those records can be merged into a single customer.
  
If both customers have previously been synced to Mailchimp, each record will already have its own **Mailchimp ID**.

During the merge process, the **Mailchimp ID** associated with the “**To**” customer (the record being kept) is **retained**, while the **Mailchimp ID** from the “**From**” customer (the record being merged) is **discarded**.  

After the merge, the connector continues to sync using the retained Mailchimp ID, ensuring that Mailchimp remains linked to the remaining customer record in Counterpoint.

### Updating an Existing Customer’s Email Address

When a customer who has already been synced to Mailchimp updates their email address in Counterpoint, the connector automatically updates the corresponding Mailchimp contact based on the stored **Mailchimp ID**. This ensures the customer’s information is preserved in Mailchimp while simply replacing the old email address with the new one.

However, if the newly entered email address in Counterpoint already exists in Mailchimp under a different contact, Mailchimp will prevent the update. In this case, an error message will appear in the Counterpoint message center, and the email address for that Mailchimp profile will remain unchanged.

---

## SECTION 6: Troubleshooting and Sync Status Codes

If a customer record is not syncing to Mailchimp, the first step is to check their **Mailchimp Sync Status** in Counterpoint.  

The sync status value indicates the current state of the customer’s record in the Mailchimp connector process.

### Sync Status Codes

| **Sync Status** | **Description** |
|------------------|-----------------|
| **0** | Customer has been synced to Mailchimp. |
| **1** | Customer will be added to Mailchimp sync queue during the next connector run. |
| **2** | Customer is currently in the active sync queue. |
| **5** | Customer has an invalid email address as determined by Mailchimp. |
| **9** | Customer encountered an error during syncing and requires investigation. |

### Viewing Sync Status in Counterpoint

To view a customer’s sync status:

1. Open the **Customer Lookup** screen in Counterpoint.  
2. Use the **Column Designer** to add the **Mailchimp Stat** column.  
   This column displays the current sync status value for each customer.

[Image Placeholder]

### Filtering by Sync Status

The **Customer Lookup** screen can also be filtered to display only customers with a specific Mailchimp status code.  

This allows for quick identification and remediation of records that are queued, invalid, or have encountered sync errors.

[Image Placeholder]

---

## SECTION 7: Importing Customers from Mailchimp into Counterpoint

The connector can be configured to import customers from Mailchimp into Counterpoint.  

This feature is especially useful for automatically creating customer records in Counterpoint when users sign up through a **website form** or other Mailchimp-integrated source.

### Limitations

The connector will **only import contacts** who belong to the **configured audience** in Mailchimp. Contacts outside of that audience will not be imported into Counterpoint.

### Caution: Avoiding Duplicate Customers

If email addresses were not previously captured in Counterpoint, the connector has no way to match existing Counterpoint records with those imported from Mailchimp.  

In this situation, **duplicate customer records** may be created. When duplicates are discovered, they can be manually merged using Counterpoint’s **Merge Customer Utility**.

This issue is particularly important for clients who use **Driver License (DL) Scan**, as duplicates can interfere with scanning workflows.  

Consult with your **Business Analyst (BA)**, **Care Team**, or **Project Manager** before enabling this functionality—especially if DL Scan is in use.

### Determining Which Fields to Import

Carefully review and select the fields to import from Mailchimp into Counterpoint. Each field can be configured individually. For best results, always enable the flag **“Retain Counterpoint Value if Mailchimp is Empty.”**

This setting prevents overwriting existing data in Counterpoint with blank or missing values from Mailchimp. For example, if **Phone 1** is populated in Counterpoint but empty in Mailchimp, the existing phone number will be preserved during import.

### Using the Template Customer

By default, the CRM_MLCHMP workgroup 230 template customer record is configured with **First Name** and **Last Name** values set to *MISSING*.
  
This ensures that if these values are not provided in the Mailchimp contact, the connector will still import the email address and create the customer record in Counterpoint.  

These imported customers can later be filtered and updated with correct names as needed.

When **Skip Merge Validation** is not enabled in the configuration, these predefined `*MISSING*` values can also be sent back to Mailchimp to populate required fields that would otherwise prevent synchronization.

### First and Last Name vs. Business/Company Name

When importing data from Mailchimp, if an existing customer record in Counterpoint is designated as a **Business** (name type = Business), the **First Name** and **Last Name** fields from Mailchimp will **not update/overwrite** the existing **business name** in Counterpoint.  

This protects business records from being replaced with individual name data during the import process.

## SECTION 8: Additional Mailchimp Tools

### Mailchimp Configuration
Configuration settings for the connector can be reviewed at any time in **Counterpoint > Connectors > Mailchimp > Mailchimp Configuration**.  

Initial settings are established prior to installation. To modify configuration values, consult with Rapid.

#### Skip Merge Validation (Configuration Option)
Some clients configure **required merge fields in Mailchimp** — often to enable required fields on a **website sign-up form**. When those fields are missing in Counterpoint, Mailchimp normally **rejects the sync** and returns an error.

Enabling **Skip Merge Validation** within the Mailchimp configuration allows the connector to **bypass merge-field validation** and **force the customer to sync**. This prevents missing fields from blocking synchronization.

This option **does not populate** missing values; it only bypasses the requirement check during sync. 

### Mailchimp Field Mapping
Customer merge fields and their mappings are displayed in **Counterpoint > Connectors > Mailchimp > Mailchimp Field Mapping**.  

This screen shows which Counterpoint fields populate Mailchimp merge fields and how each is mapped. To adjust field mappings, consult with Rapid.

### Run Mailchimp Connector (Manual Execution)
The connector runs automatically every **15 minutes**. 

For manual execution, select **Run Mailchimp Connector** in Counterpoint. A command window (black box) appears while the process runs; do not close it—the window closes automatically when the run completes.

### Mark All Mailchimp Messages as Read
If frequent pop-up alerts related to Mailchimp appear in Counterpoint, select **Mark All Mailchimp Messages as Read**.  

This action suppresses the pop-ups while keeping the messages available for later review.

## Conclusion

The Rapid Mailchimp Connector streamlines the exchange of customer and sales data between Counterpoint and Mailchimp, keeping audiences accurate, subscription preferences respected, and campaigns targeted.

Before go-live, review audience selection, merge-field mappings, and import settings (including the “Retain Counterpoint Value if Mailchimp is Empty” flag). After deployment, monitor sync status codes and resolve any data quality issues—such as invalid emails or duplicate records—to maintain accurate data.

For assistance with configuration changes, advanced mapping, or troubleshooting, contact Rapid Support.
