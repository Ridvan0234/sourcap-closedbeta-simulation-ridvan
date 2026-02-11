# Level 3: CDS Views

Now that we have data, we need to model it for the UI using Core Data Services (CDS). We will create **Interface Views** (refined data model) and **Consumption Views** (UI logic).

## Step 1: Interface Views

### 1.1 Header Interface (`ZI_Stock_Header`)
1.  Right-click `ZTOFFY_STOCK` -> **New** -> **Data Definition**.
2.  **Name**: `ZI_Stock_Header`
3.  **Description**: Stock Header Interface
4.  **Template**: Define View Entity
5.  **Copy & Paste**:

```abap
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Header Interface View'
define view entity ZI_Stock_Header
  as select from ztf_stock_hdr
  composition [0..*] of ZI_Stock_Item as _Items
{
  key snapshot_uuid as SnapshotUuid,
  plant             as Plant,
  snapshot_date     as SnapshotDate,
  total_items       as TotalItems,
  @Semantics.amount.currencyCode: 'Currency'
  total_value       as TotalValue,
  currency          as Currency,
  comment_text      as CommentText,
  
  /* Admin Fields */
  @Semantics.systemDateTime.createdAt: true
  created_at            as CreatedAt,
  @Semantics.user.createdBy: true
  created_by            as CreatedBy,
  @Semantics.systemDateTime.lastChangedAt: true
  last_changed_at       as LastChangedAt,
  @Semantics.user.lastChangedBy: true
  last_changed_by       as LastChangedBy,
  @Semantics.systemDateTime.localInstanceLastChangedAt: true
  local_last_changed_at as LocalLastChangedAt,
  
  _Items
}
```

### 1.2 Item Interface (`ZI_Stock_Item`)
1.  Right-click `ZTOFFY_STOCK` -> **New** -> **Data Definition**.
2.  **Name**: `ZI_Stock_Item`
3.  **Description**: Stock Item Interface
4.  **Template**: Define View Entity
5.  **Copy & Paste**:

```abap
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Item Interface View'
define view entity ZI_Stock_Item
  as select from ztf_stock_itm
  association to parent ZI_Stock_Header as _Header
    on $projection.SnapshotUuid = _Header.SnapshotUuid
{
  key snapshot_uuid as SnapshotUuid,
  key item_uuid     as ItemUuid,
  material_id       as MaterialId,
  material_desc     as MaterialDesc,
  storage_loc       as StorageLoc,
  @Semantics.quantity.unitOfMeasure: 'Uom'
  qty_on_hand       as QtyOnHand,
  uom               as Uom,
  @Semantics.quantity.unitOfMeasure: 'Uom'
  reorder_point     as ReorderPoint,
  risk_flag         as RiskFlag,
  @Semantics.amount.currencyCode: 'Currency'
  item_value        as ItemValue,
  currency          as Currency,
  
  case risk_flag
    when 'LOW' then 1 -- Negative (Red)
    else 3            -- Positive (Green)
  end               as RiskCriticality,
  
  /* Admin Fields */
  @Semantics.systemDateTime.createdAt: true
  created_at            as CreatedAt,
  @Semantics.user.createdBy: true
  created_by            as CreatedBy,
  @Semantics.systemDateTime.lastChangedAt: true
  last_changed_at       as LastChangedAt,
  @Semantics.user.lastChangedBy: true
  last_changed_by       as LastChangedBy,
  @Semantics.systemDateTime.localInstanceLastChangedAt: true
  local_last_changed_at as LocalLastChangedAt,
  
  _Header
}
```

## Step 2: Consumption Views (For UI)

### 2.1 Header Consumption (`ZC_Stock_Header`)
1.  New Data Definition -> Name: `ZC_Stock_Header`.
2.  **Copy & Paste**:

```abap
@EndUserText.label: 'Stock Snapshot Report'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
define root view entity ZC_Stock_Header
  provider contract transactional_query
  as projection on ZI_Stock_Header
{
  @UI.facet: [ { id: 'Header', purpose: #STANDARD, type: #IDENTIFICATION_REFERENCE, label: 'Snapshot Details', position: 10 },
               { id: 'Items', purpose: #STANDARD, type: #LINEITEM_REFERENCE, label: 'Materials', position: 20, targetElement: '_Items' } ]
  
  @UI.hidden: true
  key SnapshotUuid,

  @UI: { lineItem:       [ { position: 10, importance: #HIGH } ],
         identification: [ { position: 10 } ],
         selectionField: [ { position: 10 } ] }
  Plant,

  @UI: { lineItem:       [ { position: 20, importance: #HIGH } ],
         identification: [ { position: 20 } ],
         selectionField: [ { position: 20 } ] }
  SnapshotDate,

  @UI: { lineItem:       [ { position: 30 } ],
         identification: [ { position: 30 } ] }
  TotalItems,

  @UI: { lineItem:       [ { position: 40 } ],
         identification: [ { position: 40 } ] }
  TotalValue,

  Currency,
  CommentText,
  
  _Items : redirected to composition child ZC_Stock_Item
}
```

### 2.2 Item Consumption (`ZC_Stock_Item`)
1.  New Data Definition -> Name: `ZC_Stock_Item`.
2.  **Copy & Paste**:

```abap
@EndUserText.label: 'Stock Snapshot Item'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
define view entity ZC_Stock_Item
  as projection on ZI_Stock_Item
{
  key SnapshotUuid,
  key ItemUuid,
  
  @UI.lineItem: [ { position: 20 } ]
  MaterialId,
  
  @UI.lineItem: [ { position: 30 } ]
  MaterialDesc,
  
  @UI.lineItem: [ { position: 40 } ]
  StorageLoc,
  
  @UI.lineItem: [ { position: 50, criticality: 'RiskCriticality' } ]
  QtyOnHand,
  
  Uom,
  
  @UI.lineItem: [ { position: 60, criticality: 'RiskCriticality' } ]
  RiskFlag,
  
  @UI.hidden: true
  RiskCriticality,
  
  ItemValue,
  Currency,
  
  _Header : redirected to parent ZC_Stock_Header
}
```
**Note:** The `criticalities` annotation in `QtyOnHand` will turn the text RED if the value matches the condition associated with `criticality: #NEGATIVE`. Since `RiskFlag` holds 'LOW', we can use it to control color dynamically (advanced topic), or just display it.

---
**Views Activated!** The UI is defined.
[Next: Level 4 - Service Exposure](./04-level-4-service-exposure.md)
