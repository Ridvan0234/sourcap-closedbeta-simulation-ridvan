# Level 1: Data Model

In this level, we will set up the foundation of our application by creating the ABAP Package and the necessary Database Tables to store our Snapshot data.

## Step 1: Create a Package
1.  Right-click on **ZLOCAL** (or your preferred Super Package) -> **New** -> **ABAP Package**.
2.  **Name**: `ZTOFFY_STOCK`
3.  **Description**: Toffy Stick Snapshot Mock
4.  **Software Component**: `HOME` (or `LOCAL` for `$TMP`)
5.  **Transport Layer**: (Leave empty if `$TMP`)
6.  Click **Finish**.

## Step 2: Create Domains & Data Elements (Optional)
For simplicity, we will use standard domains or built-in types in our table definitions. In a real project, you would create `ZDE_SNAPSHOT_ID`, `ZDE_PLANT`, etc.

## Step 3: Create Database Tables
We need two tables: one for the Snapshot Header and one for the Items.

### 3.1 Header Table (`ZTF_STOCK_HDR`)
1.  Right-click on your new package `ZTOFFY_STOCK` -> **New** -> **Other ABAP Repository Object** -> **Database Table**.
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
  key client    : abap.clnt not null;
  key snapshot_uuid : sysuuid_x16 not null;
  plant         : werks_d;
  snapshot_date : datum;
  total_items   : int4;
  total_value   : netwr;
  currency      : waers;
  comment_text  : abap.string(256);
}
```
6.  **Activate** (Ctrl+F3).

### 3.2 Item Table (`ZTF_STOCK_ITM`)
1.  Right-click on package `ZTOFFY_STOCK` -> **New** -> **Other ABAP Repository Object** -> **Database Table**.
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
  key item_no       : numc6 not null;
  material_id       : matnr;
  material_desc     : maktx;
  storage_loc       : lgort_d;
  qty_on_hand       : meng13;
  uom               : meins;
  reorder_point     : meng13;
  risk_flag         : char3; // 'LOW' or 'OK'
  item_value        : netwr;
}
```
6.  **Activate** (Ctrl+F3).

---
**Congratulations!** You have created the data model.
[Next: Level 2 - Data Generation](./02-level-2-data-generation.md)
