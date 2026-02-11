# Level 2: Data Generation

In this level, we will create an ABAP class to generate mock data for our tables. Since we don't have a real S/4HANA backend in this environment, this class will simulate the "Snapshot Creation" process.

## Step 1: Create an ABAP Class
1.  Right-click on package `ZTOFFY_STOCK` -> **New** -> **ABAP Class**.
2.  **Name**: `ZCL_STOCK_GENERATOR`
3.  **Description**: Generate Mock Stock Data
4.  **Interfaces**: `if_oo_adt_classrun` (Simulate console runner)
5.  Click **Finish**.

## Step 2: Implement the Logic
**Copy & Paste** the following code into your class implementation:

```abap
CLASS zcl_stock_generator DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_oo_adt_classrun .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_stock_generator IMPLEMENTATION.

  METHOD if_oo_adt_classrun~main.
    DATA: lt_hdr TYPE TABLE OF ztf_stock_hdr,
          lt_itm TYPE TABLE OF ztf_stock_itm.

    " 1. Create a Header
    DATA(lv_uuid) = cl_system_uuid=>create_uuid_x16_static( ).
    
    " Get timestamp for RAP admin fields
    GET TIME STAMP FIELD DATA(lv_ts).

    DATA(ls_hdr) = VALUE ztf_stock_hdr(
        snapshot_uuid = lv_uuid
        plant         = '1000'
        snapshot_date = sy-datum
        total_items   = 3
        total_value   = '6200.00'
        currency      = 'EUR'
        comment_text  = 'Simulated Snapshot'
        " Admin Fields
        created_by            = sy-uname
        created_at            = lv_ts
        last_changed_by       = sy-uname
        last_changed_at       = lv_ts
        local_last_changed_at = lv_ts
    ).
    APPEND ls_hdr TO lt_hdr.

    " 2. Create Items (Simulate Material List)
    lt_itm = VALUE #(
        ( snapshot_uuid = lv_uuid item_uuid = cl_system_uuid=>create_uuid_x16_static( ) material_id = 'RM-001' material_desc = 'Raw Cocoa'   qty_on_hand = '100.000' reorder_point = '50.000'  uom = 'KG' risk_flag = 'OK'  item_value = '5000.00' currency = 'EUR' )
        ( snapshot_uuid = lv_uuid item_uuid = cl_system_uuid=>create_uuid_x16_static( ) material_id = 'RM-002' material_desc = 'Sugar'       qty_on_hand = '20.000'  reorder_point = '100.000' uom = 'KG' risk_flag = 'LOW' item_value = '200.00'  currency = 'EUR' )
        ( snapshot_uuid = lv_uuid item_uuid = cl_system_uuid=>create_uuid_x16_static( ) material_id = 'PM-001' material_desc = 'Foil Wrap'   qty_on_hand = '500.000' reorder_point = '200.000' uom = 'EA' risk_flag = 'OK'  item_value = '1000.00' currency = 'EUR' )
    ).

    " 3. Insert into DB
    DELETE FROM ztf_stock_hdr. "Reset for demo
    DELETE FROM ztf_stock_itm.

    INSERT ztf_stock_hdr FROM TABLE @lt_hdr.
    INSERT ztf_stock_itm FROM TABLE @lt_itm.

    out->write( 'Mock data generated successfully!' ).
    out->write( |Header UUID: { lv_uuid }| ).
  ENDMETHOD.
ENDCLASS.
```

## Step 3: Run the Class
1.  **Activate** the class (Ctrl+F3).
2.  Hit **F9** to run as a console application.
3.  Check the **Console** tab in Eclipse to see the "Mock data generated successfully!" message.

---
**Great work!** We now have data to display.
[Next: Level 3 - CDS Views](./03-level-3-cds-views.md)
