# Process Flow

This document describes the end-to-end operational workflow of the HR Inventory Manager system.

The process replaces a previously informal, manual workflow with a structured, digital lifecycle.

---

# High-Level Flow

The system manages the full lifecycle of a giveaway request:

1. User selects items  
2. User submits request  
3. Request is stored and notifications are triggered  
4. Request status is updated over time  
5. Delivered requests are archived  

---

# Detailed Flow

## Step 1: Inventory Interaction

- User opens the application  
- Browses available items by category  
- Adjusts quantity using inline controls  

---

## Step 2: Cart Review

- User reviews selected items  
- Validates quantities before submission  
- Confirms request  

---

## Step 3: Request Submission

- Request is written to `Giveaway_Requests`  
- Status is initialized as `Pending`  

---

## Step 4: Notification Trigger

- Power Automate detects the new request  
- Emails are sent to:
  - requester (confirmation)  
  - stock keepers (notification)  

---

## Step 5: Request Processing

- Admin or stock keeper reviews request  
- Status is updated as needed:
  - `Pending` → `Delivered`  
  - `Pending` → `Cancelled`  

---

## Step 6: Delivery Handling

When status becomes `Delivered`:

- Power Automate extracts request data  
- New record is created in `Delivered_Items`  
- request transitions from operational to historical  

---

# Lifecycle States

| Status | Meaning |
|------|--------|
| Pending | Request submitted and awaiting action |
| Delivered | Request fulfilled and completed |
| Cancelled | Request cancelled before fulfillment |

---

# Data Movement

The system ensures controlled data flow:

- Active data → `Giveaway_Requests`  
- Completed data → `Delivered_Items`  

This prevents mixing operational and historical records.

---

# Exception Handling

### Known Case: Exceeding Stock
- User is prevented from submitting invalid quantities at the UI level  

---

### Unknown Cases

Handled through:

- Power Apps error notifications  
- Power Automate run logs  
- SharePoint audit fields  

---

# Improvements Over Previous Process

### Before
- informal communication  
- inconsistent recordkeeping  
- limited visibility  

### After
- structured digital workflow  
- centralized data storage  
- automated communication  
- traceable lifecycle  

---

# Summary

The new process introduces a **clear, controlled lifecycle** for giveaway requests, ensuring:

- visibility for all stakeholders  
- reduced manual effort  
- improved data consistency  
- scalable operations  