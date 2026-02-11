# 🏭 SourcAP Closed Beta — Simulation: Stock Snapshot (RAP Delivery)

[🇹🇷 Türkçe Versiyon için tıklayın](./README_TR.md)

**Official Template Repository for SourcAP Closed Beta.**
This repository contains the starting point and instructions for your simulation task.

## 🚀 How to Complete This Simulation

Follow these steps to deliver your solution:

### 1. 📖 Read the Functional Specification
Everything you need to build (tables, objects, logic) is defined in the **Functional Specification Document**.
👉 **[Read the Functional Specification (PDF)](./docs/Toffy_Functional_Spec.pdf)**

*Note: This document contains the "Toffy Manufacturing" requirements.*

### 2. 🛠️ Environment & Project Setup
Before coding, ensure your ABAP environment is ready.
*   Open ADT (Eclipse).
*   Create a new **ABAP Cloud Project**.
*   Create a package named `Z_BETA_STOCK_<YOURNAME>`.
*   [See Setup Guide](./docs/01-environment-setup.md)

### 3. 💻 Implementation
Using the Functional Spec, build the following:
*   **Database Table**: Create the table to store stock data.
*   **CDS Views**: Interface and Consumption views.
*   **Behavior Definition**: Read-only (or standard).
*   **Service**: Service Definition and Binding.

👉 **Need step-by-step guidance?**
We have prepared a comprehensive **ABAP Guided Project** to help you with the implementation. You can follow these documents to build your solution:
*   [Step 1: Data Model](./docs/abap-guided-project/01-level-1-data-model.md)
*   [Step 2: Data Generation](./docs/abap-guided-project/02-level-2-data-generation.md)
*   [Step 3: CDS Views](./docs/abap-guided-project/03-level-3-cds-views.md)
*   [Step 4: Service Exposure](./docs/abap-guided-project/04-level-4-service-exposure.md)

Check the [docs/abap-guided-project](./docs/abap-guided-project) folder for full details.

### 4. 📤 How to Submit (SourcAP)
Once you have pushed your code to your private GitHub repo:
1.  Go to the **SourcAP Platform**.
2.  Navigate to the **Simulation Submission** area.
3.  Paste your **Repository URL**.
4.  Paste your latest **Commit Hash**.

---
**Need Help?** Check the [FAQ](./docs/04-faq.md) or open an issue in this repo.
