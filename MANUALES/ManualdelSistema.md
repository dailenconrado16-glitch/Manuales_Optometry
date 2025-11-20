---
title: "ManualdelSistema"
output: html_document
---

# 💗 SYSTEM DOCUMENTATION 💗

### Optometry System

------------------------------------------------------------------------

## 1. Project Information

**Project Name:** Optometry System\
**Student Name:** Daillent Conrado\
**Course:** System Engineer\
**Date:** 19/11/2025\
**Teacher:** Jaider Quintero\

**Short Project Description:**\
This system manages the operational workflow of an optometry center, including patient registration, appointments, visual history, optical measurements, orders, payments, deliveries, and the management of optometrists and suppliers. It provides a complete CRUD web interface using Angular as the frontend and Django as the backend API.

------------------------------------------------------------------------

## 2. System Architecture Overview

### 2.1 Architecture Description

The system follows a **client–server architecture**:

-   The **frontend** is built in Angular and runs in the user's browser.\
-   The **backend** uses Django REST Framework to expose RESTful APIs.\
-   A relational database handles persistent storage.\
-   Communication is performed via JSON over HTTP.

The system is modularized by functional domains: - Scheduling (appointments & optometrists) - Vision (patients, visual history, measurements) - Catalog (frames, lenses, orders) - Billing (payments, deliveries, suppliers)

### 2.2 Technologies Used

-   **Frontend:** Angular 16+, TypeScript, RxJS, HTML, SCSS\
-   **Backend:** Django + Django REST Framework\
-   **Database Engine:** MySQL / PostgreSQL (depending on deployment)\
-   **Additional Libraries / Tools:**
    -   PyJWT for authentication\
    -   Angular Material or Bootstrap (optional)\
    -   GitHub for version control\
    -   ERD Tools (e.g., DrawSQL / MySQL Workbench)

### 2.3 Visual Explanation of System Operation (ASCII Diagram)

![](Imagenes/Captura%20de%20pantalla%202025-11-20%20010616.png)

------------------------------------------------------------------------

## 3. Database Documentation (ENGLISH)

### 3.1 Database Description

The database supports all operations required for the optometry center. It stores patient information, appointments, optometrists, visual history, optical measurements, orders, order details, products (frames and lenses), payments, deliveries, and suppliers.

The model follows **3rd Normal Form (3NF)** and establishes clear one-to-many and many-to-one relationships.

### 3.2 ERD – Entity Relationship Diagram

![](Imagenes/3995cf9a-a012-423e-b071-f78709481f2d%20(1).jpeg)

Entities include:

\- catalog_frame\
- catalog_lens\
- catalog_order\
- catalog_orderdetail\
- billing_payment\
- billing_delivery\
- billing_supplier\
- scheduling_appointment\
- scheduling_optometrist\
- vision_patient\
- vision_visualhistory\
- vision_measurement

### 3.3 Logical Model

-   A **patient** can have multiple **visual histories**.\
-   A **visual history** can have multiple **measurements**.\
-   A **patient** can schedule multiple **appointments**, each with an assigned **optometrist**.\
-   An **order** belongs to a patient and can contain multiple **order items** (frames or lenses).\
-   An **order** may generate a **payment** and a **delivery**.\
-   A **supplier** provides frames/lenses.

### 3.4 Physical Model (Tables)

| Table             | Column     | Type    | PK/FK | Description       |
|-------------------|------------|---------|-------|-------------------|
| **catalog_frame** | id         | int     | PK    | Frame ID          |
|                   | name       | varchar |       | Name of frame     |
|                   | brand      | varchar |       | Brand             |
|                   | material   | varchar |       | Material          |
|                   | color      | varchar |       | Color             |
|                   | base_price | decimal |       | Base price        |
|                   | status     | tinyint |       | Active / inactive |

| Table        | Column      | Type    | PK/FK | Description       |
|--------------|-------------|---------|-------|-------------------|
| catalog_lens | id          | int     | PK    | Lens ID           |
| catalog_lens | name        | varchar |       | Lens name         |
| catalog_lens | brand       | varchar |       | Lens brand        |
| catalog_lens | material    | varchar |       | Material          |
| catalog_lens | description | text    |       | Lens description  |
| catalog_lens | base_price  | decimal |       | Base price        |
| catalog_lens | status      | tinyint |       | Active / inactive |

| Table         | Column         | Type    | PK/FK | Description                    |
|---------------|----------------|---------|-------|--------------------------------|
| catalog_order | id             | int     | PK    | Order ID                       |
| catalog_order | order_date     | date    |       | Order date                     |
| catalog_order | total_amount   | decimal |       | Order total                    |
| catalog_order | status         | varchar |       | pending / approved / delivered |
| catalog_order | appointment_id | int     | FK    | Links to appointment           |
| catalog_order | patient_id     | int     | FK    | Links to patient               |

| Table               | Column       | Type    | PK/FK | Description       |
|---------------------|--------------|---------|-------|-------------------|
| catalog_orderdetail | id           | int     | PK    | Item ID           |
| catalog_orderdetail | product_type | enum    |       | 'frame' or 'lens' |
| catalog_orderdetail | quantity     | int     |       |                   |
| catalog_orderdetail | unit_price   | decimal |       |                   |
| catalog_orderdetail | graduation   | varchar |       | Lens degree       |
| catalog_orderdetail | axis         | varchar |       | Axis              |
| catalog_orderdetail | frame_id     | int     | FK    |                   |
| catalog_orderdetail | lens_id      | int     | FK    |                   |
| catalog_orderdetail | order_id     | int     | FK    |                   |

| Table          | Column      | Type    | PK/FK | Description |
|----------------|-------------|---------|-------|-------------|
| vision_patient | id          | int     | PK    |             |
| vision_patient | first_name  | varchar |       |             |
| vision_patient | last_name   | varchar |       |             |
| vision_patient | document_id | varchar |       |             |
| vision_patient | phone       | varchar |       |             |
| vision_patient | email       | varchar |       |             |
| vision_patient | address     | varchar |       |             |
| vision_patient | birth_date  | date    |       |             |
| vision_patient | status      | tinyint |       |             |

| Table                | Column     | Type    | PK/FK | Description |
|----------------------|------------|---------|-------|-------------|
| vision_visualhistory | id         | int     | PK    |             |
| vision_visualhistory | patient_id | int     | FK    |             |
| vision_visualhistory | notes      | text    |       |             |
| vision_visualhistory | created_at | date    |       |             |
| vision_visualhistory | status     | tinyint |       |             |

| Table              | Column             | Type    | PK/FK | Description |
|--------------------|--------------------|---------|-------|-------------|
| vision_measurement | id                 | int     | PK    |             |
| vision_measurement | sphere             | decimal |       |             |
| vision_measurement | cylinder           | decimal |       |             |
| vision_measurement | axis               | int     |       |             |
| vision_measurement | pupillary_distance | int     |       |             |
| vision_measurement | date               | date    |       |             |
| vision_measurement | status             | tinyint |       |             |
| vision_measurement | visual_history_id  | int     | FK    |             |

| Table                  | Column         | Type    | PK/FK | Description |
|------------------------|----------------|---------|-------|-------------|
| scheduling_optometrist | id             | int     | PK    |             |
| scheduling_optometrist | first_name     | varchar |       |             |
| scheduling_optometrist | last_name      | varchar |       |             |
| scheduling_optometrist | license_number | varchar |       |             |
| scheduling_optometrist | phone          | varchar |       |             |
| scheduling_optometrist | email          | varchar |       |             |
| scheduling_optometrist | status         | tinyint |       |             |

| Table                  | Column             | Type     | PK/FK | Description      |
|------------------------|--------------------|----------|-------|------------------|
| scheduling_appointment | id                 | int      | PK    | Appointment ID   |
| scheduling_appointment | date_time          | datetime |       | Appointment date |
| scheduling_appointment | reason             | text     |       | Reason           |
| scheduling_appointment | notes              | text     |       | Notes            |
| scheduling_appointment | appointment_status | varchar  |       | Status           |
| scheduling_appointment | patient_id         | int      | FK    |                  |
| scheduling_appointment | optometrist_id     | int      | FK    |                  |

| Table           | Column       | Type    | PK/FK | Description |
|-----------------|--------------|---------|-------|-------------|
| billing_payment | id           | int     | PK    |             |
| billing_payment | amount       | decimal |       |             |
| billing_payment | payment_date | date    |       |             |
| billing_payment | method       | varchar |       |             |
| billing_payment | status       | varchar |       |             |
| billing_payment | order_id     | int     | FK    |             |

| Table            | Column         | Type    | PK/FK | Description |
|------------------|----------------|---------|-------|-------------|
| billing_delivery | id             | int     | PK    |             |
| billing_delivery | tracking_code  | varchar |       |             |
| billing_delivery | estimated_date | date    |       |             |
| billing_delivery | delivered_date | date    |       |             |
| billing_delivery | status         | varchar |       |             |
| billing_delivery | order_id       | int     | FK    |             |
| billing_delivery | supplier_id    | int     | FK    |             |

| Table            | Column  | Type    | PK/FK | Description |
|------------------|---------|---------|-------|-------------|
| billing_supplier | id      | int     | PK    |             |
| billing_supplier | name    | varchar |       |             |
| billing_supplier | phone   | varchar |       |             |
| billing_supplier | email   | varchar |       |             |
| billing_supplier | address | varchar |       |             |
| billing_supplier | status  | tinyint |       |             |

------------------------------------------------------------------------

## 4. Use Cases – CRUD

### 4.1 Use Case: Create Patient

**Actor:** Optometry Staff\
**Description:** Registers a new patient into the system.\
**Preconditions:** Staff is authenticated.\
**Postconditions:** Patient is stored in database.

**Main Flow:**\
1. User opens “Create Patient” form.\
2. User enters required fields.\
3. System validates input.\
4. Django API stores patient.\
5. Angular displays success message.

### 4.2 Use Case: Read Patient

-   Angular requests list from `/api/patients/`.\
-   Django returns JSON collection.

### 4.3 Use Case: Update Patient

-   User selects a patient and updates information.\
-   Django performs PUT request validation.

### 4.4 Use Case: Delete Patient

-   User confirms deletion.\
-   Django deletes record (soft delete is recommended).

*(Same CRUD logic applies to appointments, orders, suppliers, payments, measurement, visual history, optometrists)*

------------------------------------------------------------------------

## 5. Backend Documentation

### 5.1 Backend Architecture

The backend uses Django’s layered organization:

-   **Models:** Database entities\
-   **Serializers:** JSON conversion\
-   **Views:** Controller logic\
-   **URLs:** Route configuration\
-   **DRF:** REST API handling

### 5.2 Folder Structure

![](Imagenes/Captura%20de%20pantalla%202025-11-20%20010840.png)

### 5.3 API Documentation (REST)

**GET /api/patients/**\
Returns list of patients.

**POST /api/patients/**\
Creates a new patient.

**PUT /api/patients/{id}**\
Updates patient info.

**DELETE /api/patients/{id}**\
Deletes patient.

*(Same structure for: appointments, orders, deliveries, payments, etc.)*

Example JSON request body:

\`\`\`json { "first_name": "Carlos", "last_name": "Gomez", "document_id": "112233", "phone": "3124567890", "email": "[carlos\@example.com](mailto:carlos@example.com){.email}" }

### **5.4 REST Client**

## Tested using Postman or Django REST Browsable API.

### **6.Frontend Documentation 6.1 Technical Frontend Documentation**

Framework Used: Angular 16+ Programming Language: TypeScript Architecture: Modular, Service-based, Reactive Forms

**Folder Structure:**

![](Imagenes/Captura%20de%20pantalla%202025-11-20%20010937.png)

### 6.2 Visual Explanation

## The Angular UI presents CRUD tables and reactive forms connected to Django API services.

### 7.Frontend–Backend Integration

Angular service calls Django REST endpoints via HttpClient.

Django returns JSON.

Angular displays data in tables, forms, or detail views.

## All operations follow REST conventions.

### 8. Conclusions & Recommendations

The system provides full optometry workflow automation.

Clear modularization allows scaling the system easily.

Adding authentication (JWT) is recommended.

## Future improvement: Report generation, dashboards, and role-based permissions.

### ¡Gracias! 💗
