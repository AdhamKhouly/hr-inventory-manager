# Power Apps

This folder contains the frontend application for the HR Inventory Manager, built using **Microsoft Power Apps (Canvas App)**.

The application serves as the primary interface for both employees and administrators, enabling end-to-end interaction with the giveaway inventory system.

---

# Application Overview

The Power Apps solution provides a unified interface for:

- browsing available giveaway items
- selecting and requesting items
- managing request quantities
- handling request lifecycle interactions
- supporting both standard users and admins

The application is designed to be **fully responsive** and accessible across multiple device types.

---

# Application Experience

## Dual Interface Design

The app supports two primary user experiences:

### 1. Standard User View
- browse items by category
- adjust quantities using inline controls
- add items to cart
- submit giveaway requests
- view cart before submission

### 2. Admin View
- extended controls for managing requests
- additional actions on items and requests
- visibility into request lifecycle states

The interface dynamically adapts based on the user’s role.

---

## Responsive Layout

The application is designed to work seamlessly across:

- desktop
- tablet
- mobile devices

Key UI considerations include:

- adaptive layouts for different screen sizes
- touch-friendly controls for mobile interaction
- consistent user experience across devices

---

# Core UI Components

The application is structured around a set of reusable UI patterns:

### Category Navigation
Users can filter inventory using predefined categories:

- Bags  
- Boxes  
- Calculators  
- Cardholders  
- Key Chains  
- Medals  
- Mugs  
- Notebooks  
- Pens  
- Pins  
- Power Banks  
- Speakers  

---

### Inventory Gallery

Displays available giveaway items with:

- item image
- item name
- category
- available quantity (implicitly controlled)
- quantity adjustment controls

---

### Quantity Controls

Each item includes:

- increment (`+`) control
- decrement (`–`) control
- real-time quantity feedback

These controls enforce user interaction directly within the gallery.

---

### Cart Interaction

- items can be added directly from the gallery
- users can review selections before submission
- "View Cart" acts as the transition to request confirmation

---

### Request Submission

Once confirmed:

- request is sent to SharePoint
- triggers downstream automation workflows
- initiates notification process

---

# Data Integration

The application is tightly integrated with SharePoint as its backend.

### Connected Lists

- `Gift_Stocks` → inventory source  
- `Giveaway_Requests` → request storage  
- `Admins` → role-based access control  
- `stock_keepers` → notification recipients  

All interactions are handled through Power Apps data connections.

---

# Role-Based Behavior

Role-based logic is implemented using user identity (email) and SharePoint lists.

### Behavior Differences

| Capability | Standard User | Admin |
|----------|-------------|------|
| Browse items | ✓ | ✓ |
| Submit requests | ✓ | ✓ |
| View extended controls | ✗ | ✓ |
| Manage request flow | ✗ | ✓ |

This ensures controlled access without duplicating application logic.

---

# Design Principles

The application was built with the following principles:

- **Simplicity**: minimal steps to complete a request  
- **Clarity**: clear visibility of available items  
- **Responsiveness**: consistent behavior across devices  
- **Efficiency**: inline interactions reduce navigation overhead  
- **Scalability**: supports growing inventory and request volume  

---

# Files

## exports/

Contains the exported Power Apps application:

- `.msapp` or `.zip` file representing the full app

---

## screenshots/

Contains UI representations of the application:

- `normal-view.png` → standard user interface  
- `admin-view.png` → admin interface  

---

# Notes

- The application is built as a **Canvas App** using standard Power Apps components  
- No premium connectors are required  
- All business logic is implemented through:
  - Power Apps formulas
  - SharePoint data interactions  
  - Power Automate workflows (see `../powerautomate/`)  

---

# Summary

The Power Apps layer serves as the **user-facing entry point** of the HR Inventory Manager system, translating business requirements into an intuitive, responsive, and role-aware interface.

It demonstrates how complex operational workflows can be implemented using a low-code platform while maintaining usability, control, and scalability.