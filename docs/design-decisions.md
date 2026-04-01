# Design Decisions

This document outlines the key design choices made during the development of the HR Inventory Manager, along with their rationale and tradeoffs.

---

# 1. Use of SharePoint as Backend

## Decision
Use SharePoint Lists as the primary data store.

## Rationale
- native integration with Power Apps and Power Automate  
- built-in permissions and auditability  
- no additional infrastructure required  

## Tradeoff
- limited relational capabilities compared to traditional databases  

---

# 2. Separation of Active and Archived Data

## Decision
Split data between:
- `Giveaway_Requests` (active)  
- `Delivered_Items` (archived)  

## Rationale
- improves performance  
- simplifies queries  
- supports long-term scalability  

## Tradeoff
- controlled duplication of data  

---

# 3. Event-Driven Automation

## Decision
Trigger workflows based on SharePoint changes.

## Rationale
- real-time responsiveness  
- eliminates need for polling  
- reduces unnecessary executions  

## Tradeoff
- requires careful trigger design to avoid duplicate runs  

---

# 4. Role-Based Access via Lists

## Decision
Store roles in SharePoint (`Admins`, `Stock_Keepers`).

## Rationale
- dynamic and easily configurable  
- avoids hardcoding roles in the app  
- enables non-technical updates  

## Tradeoff
- requires additional lookup logic in Power Apps  

---

# 5. Inline Interaction Design

## Decision
Allow quantity selection and interaction directly in the gallery.

## Rationale
- reduces navigation steps  
- improves usability  
- speeds up request process  

## Tradeoff
- increases UI logic complexity  

---

# 6. Minimal Dependency Architecture

## Decision
Use only standard Microsoft connectors.

## Rationale
- avoids licensing overhead  
- simplifies deployment  
- improves maintainability  

## Tradeoff
- limits integration with external systems  

---

# 7. Archiving via Workflow Instead of Deletion

## Decision
Move delivered requests instead of deleting them.

## Rationale
- preserves historical data  
- supports reporting and auditing  
- maintains system transparency  

## Tradeoff
- requires additional storage  

---

# 8. Metadata-Based Auditability

## Decision
Rely on SharePoint system fields for tracking.

## Rationale
- automatic tracking of changes  
- no need for custom logging system  

## Tradeoff
- limited customization of audit fields  

---

# Summary

The system design prioritizes:

- simplicity over unnecessary complexity  
- scalability through data separation  
- maintainability through low-code patterns  
- flexibility through configuration-driven logic  

These decisions collectively enable a **robust, scalable, and production-ready solution using the Microsoft Power Platform**.