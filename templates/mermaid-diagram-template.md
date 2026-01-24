# Architecture Diagram (edit for your solution)

```mermaid
graph LR
  User[User] --> UI[Fiori/UI Preview]
  UI --> OData[OData Service]
  OData --> SD[Service Definition]
  SD --> C[Consumption View]
  C --> I[Interface View]
  I --> DB[(DB / Source)]
```
