# Level 4: Service Exposure

The final step is to expose our Consumptions Views as an **OData Service** so a Fiori app can consume them.

## Step 1: Service Definition
1.  Right-click `ZTOFFY_STOCK` -> **New** -> **Other** -> **Service Definition**.
2.  **Name**: `ZUI_STOCK_SRV`
3.  **Description**: Stock Service Definition
4.  **Copy & Paste**:

```abap
@EndUserText.label: 'Stock Report Service'
define service ZUI_STOCK_SRV {
  expose ZC_Stock_Header as StockHeader;
  expose ZC_Stock_Item   as StockItem;
}
```
5.  **Activate** (Ctrl+F3).

## Step 2: Service Binding
1.  Right-click on `ZUI_STOCK_SRV` (the definition) -> **New Service Binding**.
2.  **Name**: `ZUI_STOCK_BINDING`
3.  **Binding Type**: `OData V2 - UI` (Standard for Fiori Elements).
4.  Click **Finish**.
5.  In the editor that opens, click **Activate**.

## Step 3: Preview the App
1.  Once activated, look at the **Service URL** section on the right.
2.  Select `StockHeader` entity set.
3.  Click the **Preview** button (browser icon).
4.  Standard Fiori Elements List Report will open in your browser.
5.  Click **Go** to see the data generated in Level 2!

---
**Congratulations!** You have built a full ABAP RAP application from scratch.

### What's Next?
-   Implement the logic to calculate `RiskFlag` dynamically in a Determination.
-   Add a "Generate Snapshot" button using a UI Action.
