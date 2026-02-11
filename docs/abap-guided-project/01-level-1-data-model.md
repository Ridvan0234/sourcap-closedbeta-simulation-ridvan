# Level 1: Data Model

In this level, we will set up the foundation of our application by creating the ABAP Package and the necessary Database Tables to store our Snapshot data.

## Step 1: Create a Package
1.  Right-click on **ZLOCAL** (or your preferred Super Package) -> **New** -> **ABAP Package**.
2.  **Name**: `ZSCP_BETA_STOCK_YOURNAME`
3.  **Description**: Stock Snapshot Mock Application
4.  **Software Component**: `HOME` (or `LOCAL` for `$TMP`)
5.  **Transport Layer**: (Leave empty if `$TMP`)
6.  Click **Finish**.

## Step 2: Create Domains & Data Elements (Optional)
For simplicity, we will use standard domains or built-in types in our table definitions. In a real project, you would create `ZDE_SNAPSHOT_ID`, `ZDE_PLANT`, etc.

## Step 3: Create Database Tables
We need two tables: one for the Snapshot Header and one for the Items. We will use modern ABAP types and include standard RAP administrative fields.

### 3.1 Header Table (`ZTF_STOCK_HDR`)
1.  Right-click on your new package `ZSCP_BETA_STOCK_YOURNAME` -> **New** -> **Other ABAP Repository Object** -> **Database Table**.
2.  **Name**: `ZTF_STOCK_HDR`
3.  **Description**: Stock Snapshot Header
4.  Click **Finish**.
5.  **Copy & Paste** the following code into the editor:

```abap
@EndUserText.label : 'Stock Snapshot Header'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table ztf_stock_hdr {
  key client            : abap.clnt not null;
  key snapshot_uuid     : sysuuid_x16 not null;
  plant                 : abap.char(4);
  snapshot_date         : abap.dats; 
  total_items           : abap.int4; 
  @Semantics.amount.currencyCode : 'currency'
  total_value           : abap.curr(15,2);
  currency              : abap.cuky;
  comment_text          : abap.string(256);
  
  -- Admin Fields
  created_by            : abp_creation_user;
  created_at            : abp_creation_tstmpl;
  last_changed_by       : abp_lastchange_user;
  last_changed_at       : abp_lastchange_tstmpl;
  local_last_changed_at : abp_locinst_lastchange_tstmpl;
}
```
6.  **Activate** (Ctrl+F3).

### 3.2 Item Table (`ZTF_STOCK_ITM`)
1.  Right-click on package `ZSCP_BETA_STOCK_YOURNAME` -> **New** -> **Other ABAP Repository Object** -> **Database Table**.
2.  **Name**: `ZTF_STOCK_ITM`
3.  **Description**: Stock Snapshot Item
4.  Click **Finish**.
5.  **Copy & Paste** the following code:

```abap
@EndUserText.label : 'Stock Snapshot Item'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table ztf_stock_itm {
  key client        : abap.clnt not null;
  key snapshot_uuid : sysuuid_x16 not null;
  key item_uuid     : sysuuid_x16 not null;
  material_id       : abap.char(18);
  material_desc     : abap.char(40);
  storage_loc       : abap.char(4);
  @Semantics.quantity.unitOfMeasure : 'uom'
  qty_on_hand       : abap.quan(13,3);
  uom               : abap.unit(3);
  @Semantics.quantity.unitOfMeasure : 'uom'
  reorder_point     : abap.quan(13,3);
  risk_flag         : abap.char(3);
  @Semantics.amount.currencyCode : 'currency'
  item_value        : abap.curr(15,2);
  currency          : abap.cuky;
  
  -- Admin Fields
  created_by            : abp_creation_user;
  created_at            : abp_creation_tstmpl;
  last_changed_by       : abp_lastchange_user;
  last_changed_at       : abp_lastchange_tstmpl;
  local_last_changed_at : abp_locinst_lastchange_tstmpl;
}
```
6.  **Activate** (Ctrl+F3).

---
**Congratulations!** You have created the data model.
[Next: Level 2 - Data Generation](./02-level-2-data-generation.md)
