# Toffy ABAP Implementation Guide - Overview

Welcome to the **Toffy Stock Snapshot Report** guided project. This series of documents will guide you step-by-step through implementing the backend logic for the Toffy Manufacturing "Stock Snapshot" report using SAP BTP ABAP Environment.

## Scenario
We are building a report to allow inventory analysts to:
1.  **Generate** a snapshot of current stock levels (simulated).
2.  **View** a list of snapshots.
3.  **Analyze** details of a specific snapshot, identifying items with low stock (Risk = LOW).

## Architecture
We will use the **ABAP RESTful Application Programming Model (RAP)**.

1.  **Level 1: Data Model**: Database tables to store snapshot headers and items.
2.  **Level 2: Business Logic**: An ABAP class to generate simulated data.
3.  **Level 3: CDS Views**: Interface and Consumption views to model the data for the UI.
4.  **Level 4: Service Exposure**: Service Definition and Binding to expose the data as an OData service for a Fiori Elements App.

## Prerequisites
-   **Eclipse IDE** with **ADT (ABAP Development Tools)** installed.
-   Access to an **SAP BTP ABAP Environment** (Trial or Enterprise).
-   A valid generic Development Package (e.g., `ZTOFFY_STOCK` or `$TMP`).

## conventions
-   **Package**: `ZSCP_BETA_STOCK_YOURNAME` (Replace `YOURNAME` with your name/initials).
-   **Prefix**: `ZTF_` for tables, `ZI_` for interface views, `ZC_` for consumption views.

---
[Next: Level 1 - Data Model](./01-level-1-data-model.md)
