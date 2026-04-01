# Power Automate

This folder documents the automation layer of the HR Inventory Manager, implemented using **Microsoft Power Automate**.

The workflows are designed to be **event-driven**, **stateless**, and tightly integrated with SharePoint to manage the lifecycle of giveaway requests with minimal manual intervention.

---

# Automation Overview

Power Automate acts as the orchestration layer of the system, handling:

- request lifecycle transitions  
- stakeholder communication  
- data movement between operational and archival structures  

The flows are triggered directly from SharePoint events, ensuring real-time responsiveness without the need for polling or scheduled jobs.

---

# Workflow Architecture

The automation layer is built around two core workflows:

1. **Request Notification Flow**
2. **Delivery Archiving Flow**

Each flow is designed to handle a specific stage in the request lifecycle, maintaining clear separation of responsibilities.

---

# Flow 1: Request Notification

## Trigger

- Activated on **creation or modification** of items in the `Giveaway_Requests` SharePoint list

## Objective

- Standardize communication across the request lifecycle  
- Eliminate reliance on manual email coordination  
- Ensure visibility for both requesters and operational stakeholders  

## Logic

1. Detect a new or updated request  
2. Evaluate the `Status` field:
   - `Pending`
   - `Cancelled`
3. Identify relevant stakeholders:
   - requester (Created By)
   - stock keepers (from `stock_keepers` list)  
4. Generate structured email notifications based on request state  

## Behavior

- **Pending**
  - confirmation email sent to requester  
  - notification sent to stock keepers  

- **Cancelled**
  - cancellation confirmation sent to requester  
  - optional notification to stakeholders  

## Outcome

- consistent and automated communication  
- improved transparency of request activity  
- reduced dependency on manual follow-ups  

---

# Flow 2: Delivery Archiving

## Trigger

- Activated when the `Status` field in `Giveaway_Requests` is updated to **Delivered**

## Objective

- Separate active requests from completed records  
- preserve historical data for auditing and reporting  
- prevent long-term performance degradation of the main list  

## Logic

1. Detect transition of request status → `Delivered`  
2. Extract relevant fields:
   - giveaway name  
   - requester details  
   - request and delivery timestamps  
   - item reference (Gift ID)  
3. Create a new record in the `Delivered_Items` SharePoint list  
4. Maintain traceability between original request and archived record  

## Outcome

- completed requests are stored in a dedicated archive  
- active request list remains lightweight and performant  
- historical data is preserved in a structured format  

---

# Design Principles

The automation layer follows a set of deliberate design principles:

### Event-Driven Execution
Flows are triggered directly by SharePoint changes, ensuring immediate and efficient execution.

---

### Separation of Concerns
- operational data (`Giveaway_Requests`)  
- historical data (`Delivered_Items`)  

This separation improves maintainability and scalability.

---

### Idempotent Behavior
Flows are structured to avoid unintended duplication or inconsistent states when triggered multiple times.

---

### Minimal Coupling
Flows rely only on:
- SharePoint lists  
- Outlook email connectors  

No external dependencies or premium connectors are required.

---

### Scalability Considerations

- archiving prevents list growth from degrading performance  
- flows operate on individual events, avoiding batch processing overhead  
- logic remains simple and maintainable as volume increases  

---

# Integration with System Layers

| Layer | Responsibility |
|------|--------------|
| Power Apps | user interaction and request submission |
| Power Automate | lifecycle orchestration and communication |
| SharePoint | persistent data storage |

Power Automate serves as the **bridge between user actions and backend state transitions**.

---

# Error Handling Strategy

The flows are designed to handle common failure scenarios:

- SharePoint connectivity issues  
- incomplete or invalid data states  
- email delivery failures  

In such cases:
- execution logs are available within Power Automate  
- failed runs can be retried without affecting system integrity  

---

# Security and Permissions

- flows run under controlled Microsoft 365 connections  
- access to SharePoint lists is governed by organizational permissions  
- email distribution is limited to configured stakeholders  

---

# Summary

The Power Automate layer transforms the HR Inventory Manager from a static application into a **fully automated workflow system**.

It ensures that:

- requests are consistently tracked  
- stakeholders are automatically informed  
- completed transactions are properly archived  

This demonstrates how **low-code automation can be used to implement reliable, scalable business process orchestration** within an enterprise environment.