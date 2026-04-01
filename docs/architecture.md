# System Architecture

This document describes the high-level architecture of the HR Inventory Manager, built using the **Microsoft Power Platform**.

The system follows a modular, layered architecture that separates user interaction, automation, and data persistence.

---

# Architecture Overview

The system is composed of three primary layers:

1. **Presentation Layer** – Power Apps  
2. **Automation Layer** – Power Automate  
3. **Data Layer** – SharePoint  

Each layer is independently responsible for a specific concern while remaining tightly integrated.

---

# Layered Architecture

## 1. Presentation Layer (Power Apps)

The Power Apps Canvas application serves as the user-facing interface.

### Responsibilities

- display available inventory  
- allow users to select and request items  
- enforce UI-level constraints (e.g., quantity selection)  
- adapt interface based on user role (admin vs standard user)  

### Characteristics

- responsive design across devices  
- minimal navigation friction through inline interactions  
- real-time data binding to SharePoint  

---

## 2. Automation Layer (Power Automate)

Power Automate orchestrates the system’s business logic and lifecycle transitions.

### Responsibilities

- trigger workflows based on SharePoint events  
- send notifications to stakeholders  
- handle request lifecycle transitions  
- move completed records into archive storage  

### Characteristics

- event-driven execution  
- stateless workflows  
- minimal dependency on external systems  

---

## 3. Data Layer (SharePoint)

SharePoint Lists act as the system’s persistent storage.

### Responsibilities

- store inventory data  
- store active requests  
- maintain role-based access lists  
- store historical delivery records  

### Characteristics

- structured but flexible schema  
- built-in auditability via metadata  
- optimized through separation of active and archived data  

---

# Interaction Flow Between Layers

The system operates through tightly coupled but clearly separated interactions:

1. User interacts with Power Apps  
2. Power Apps writes to SharePoint  
3. SharePoint triggers Power Automate workflows  
4. Power Automate processes logic and updates data  
5. Updated data is reflected back in Power Apps  

This creates a closed-loop system with real-time feedback.

---

# Key Architectural Decisions

### Event-Driven Design
All business logic is triggered by data changes rather than scheduled jobs.

---

### Separation of Active vs Historical Data
- `Giveaway_Requests` → active operations  
- `Delivered_Items` → archived records  

This ensures long-term performance and maintainability.

---

### Role-Based UI Control
User roles are determined dynamically via SharePoint lists, avoiding hardcoded logic.

---

### Low-Code, High-Leverage Approach
The system maximizes capabilities of the Microsoft ecosystem without requiring custom backend infrastructure.

---

# Scalability Considerations

- SharePoint list growth is controlled via archiving  
- workflows scale with event volume rather than batch size  
- UI complexity remains manageable through component reuse  

---

# Reliability Considerations

- workflows rely on built-in Microsoft connectors  
- SharePoint provides data consistency and audit trails  
- system avoids complex dependencies that increase failure risk  

---

# Summary

The HR Inventory Manager architecture demonstrates how a **low-code platform can be used to build a structured, scalable, and maintainable business application**.

By separating presentation, automation, and data concerns, the system achieves:

- clarity of design  
- ease of maintenance  
- scalability over time  