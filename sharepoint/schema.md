# SharePoint Data Model

This document defines the data layer of the HR Inventory Manager, implemented using **SharePoint Lists**.

The data model is designed to support:

- real-time inventory tracking  
- request lifecycle management  
- role-based access control  
- historical record preservation  

The structure separates **operational data** from **archival data**, ensuring scalability and long-term maintainability.

---

# Overview

The system is built on five core SharePoint lists:

1. `Gift_Stocks`  
2. `Giveaway_Requests`  
3. `Admins`  
4. `Stock_Keepers`  
5. `Delivered_Items`  

Each list serves a distinct role within the system architecture.

---

# Data Architecture Principles

The schema follows several key design principles:

### Separation of Concerns
- Active requests are stored in `Giveaway_Requests`  
- Completed requests are moved to `Delivered_Items`  

This prevents uncontrolled list growth and improves performance.

---

### Minimal Redundancy
Data is stored only where necessary, with controlled duplication in the archive layer for historical traceability.

---

### Event Compatibility
The schema is structured to align with **Power Automate triggers**, enabling clean event-driven workflows.

---

### Auditability
All lists retain system metadata:
- Created  
- Modified  
- Created By  
- Modified By  

This ensures traceability of all actions.

---

# Entity Definitions

## 1. Gift_Stocks

### Purpose
Stores the current inventory of available giveaway items.

### Structure

| Column | Type | Description |
|---|---|---|
| Item | Single line of text | Name of the giveaway item |
| Category | Choice | Predefined classification of items |
| Quantity | Number | Current available stock |
| Created | Date & Time | Record creation timestamp |
| Modified | Date & Time | Last update timestamp |
| Created By | Person or Group | Record creator |
| Modified By | Person or Group | Last modifier |

### Notes
- Serves as the **source of truth** for inventory  
- Quantity is dynamically referenced during request validation  
- Category enables UI-level filtering and grouping  

---

## 2. Giveaway_Requests

### Purpose
Stores all active and in-progress giveaway requests.

### Structure

| Column | Type | Description |
|---|---|---|
| Giveaway_ordered | Single line of text | Name of the requested item |
| Requested_Quantity | Number | Quantity requested by the user |
| Status | Choice | Pending, Delivered, Cancelled |
| Gift ID | Number | Reference to item in `Gift_Stocks` |
| Created | Date & Time | Request submission timestamp |
| Modified | Date & Time | Last update timestamp |
| Created By | Person or Group | Requester |
| Modified By | Person or Group | Last modifier |

### Notes
- Acts as the **operational core** of the system  
- Drives Power Automate workflows through status changes  
- Status transitions define the request lifecycle  

---

## 3. Admins

### Purpose
Defines administrative users with extended privileges.

### Structure

| Column | Type |
|---|---|
| Email | Single line of text |

### Notes
- Used for role-based UI logic in Power Apps  
- Enables dynamic control without hardcoding permissions  

---

## 4. Stock_Keepers

### Purpose
Stores the list of stakeholders responsible for inventory fulfillment.

### Structure

| Column | Type |
|---|---|
| Email | Single line of text |

### Notes
- Used by Power Automate to distribute notifications  
- Decouples communication logic from application code  

---

## 5. Delivered_Items

### Purpose
Stores archived records of completed giveaway requests.

### Structure

| Column | Type | Description |
|---|---|---|
| Delivery ID | Number | Unique delivery record identifier |
| Giveaway name | Single line of text | Delivered item name |
| date ordered | Date & Time | Original request date |
| date delivered | Date & Time | Fulfillment timestamp |
| Gift ID | Number | Reference to inventory item |
| Requester Name | Single line of text | Name of requester |
| Requester Email | Single line of text | Requester email |
| Created | Date & Time | Record creation timestamp |
| Modified | Date & Time | Last update timestamp |
| Created By | Person or Group | System metadata |
| Modified By | Person or Group | System metadata |

### Notes
- Functions as a **historical archive layer**  
- Populated via Power Automate upon delivery completion  
- Preserves key data for reporting and auditing  

---

# Relationships

Although SharePoint does not enforce strict relational constraints, logical relationships exist:

- `Giveaway_Requests.Gift ID` → `Gift_Stocks`  
- `Delivered_Items.Gift ID` → `Gift_Stocks`  

These relationships are maintained at the application and workflow level.

---

# Data Flow

The lifecycle of data across the system:

1. Inventory is maintained in `Gift_Stocks`  
2. User submits request → stored in `Giveaway_Requests`  
3. Status updated over time (`Pending` → `Delivered` / `Cancelled`)  
4. On delivery:
   - record is copied to `Delivered_Items`  
   - request is effectively transitioned to archive  

This flow ensures clean separation between **active operations** and **historical records**.

---

# Design Tradeoffs

### Why not store everything in one list?
- SharePoint performance degrades with large lists  
- mixing active and historical data complicates queries  

---

### Why duplicate data in Delivered_Items?
- ensures historical records remain immutable  
- avoids dependency on mutable operational data  

---

### Why use Email-based role lists?
- enables dynamic access control  
- avoids hardcoding user roles in the application  

---

# Scalability Considerations

- Archiving strategy prevents long-term performance issues  
- Event-driven flows scale linearly with usage  
- Schema supports increasing inventory and request volume  

---

# Summary

The SharePoint schema provides a **lightweight but structured data layer** that enables:

- real-time inventory visibility  
- controlled request lifecycle management  
- automated workflow integration  
- scalable historical tracking  

It demonstrates how SharePoint can be used as an effective backend for enterprise-grade process automation when combined with Power Apps and Power Automate.