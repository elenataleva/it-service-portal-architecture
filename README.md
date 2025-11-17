# IT Service Request Portal – Architecture & PoC

## 1. Overview

Modern IT teams are flooded with ad-hoc messages on email, chat, and phone when something breaks or an employee needs access to a system. Requests get lost, there is no clear ownership, and managers have no visibility into what IT is actually working on.

The **IT Service Request Portal** is a simple ticketing system where:
- **Employees** can create service requests (e.g. “my VPN doesn’t work”, “I need access to Jira”), track their status, and see previous tickets.
- **IT agents** can view and manage tickets, update status, add comments, and resolve issues.
- **Managers** can get a basic overview of volume and status of requests.

This project is a **small architecture and PoC implementation**, inspired by ITSM tools like ServiceNow. The focus is on:
- Clear **layered architecture** (presentation, API, service, data, integrations).
- A simple **Java / Spring Boot backend**.
- Documentation written as if this were a **solution proposal for a customer**.

---

## 2. Problem & Goals

**Problem:**  
IT support is often handled via unstructured channels. There is no single place where employees can submit requests and see status, and IT teams lack a lightweight tool to triage and track work.

**Goal:**  
Design and implement a **minimal IT Service Request Portal** that:
- Centralizes employee requests in one place.
- Enables IT to manage ticket lifecycle (open → in progress → resolved).
- Demonstrates a clean architecture that could be extended or integrated with systems like ServiceNow in the future.

---

## 3. Requirements

### 3.1 Functional Requirements

**Employee-facing:**
- Create a new ticket with:
    - Title
    - Description
    - Category (e.g. Access, Incident, Hardware, Other)
    - Priority (**Minor / Major / Critical**)
    - ReportedBy (email or name)
- View a list of **my tickets**.
- See ticket details and current status.

**IT agent-facing (basic for PoC):**
- View **all tickets**.
- Update ticket status (`OPEN`, `IN_PROGRESS`, `RESOLVED`, `CANCELLED`).
- Add internal or public comments to a ticket.

**System / generic:**
- Each ticket must have:
    - Unique ID
    - Created date/time
    - Last updated date/time
    - Created-by user reference

---

### 3.2 Non-Functional Requirements

- **Simplicity:** easy to run locally using Spring Boot + H2.
- **Layered Architecture:** Presentation → API → Service → Data → Integration.
- **Extensibility:** adding new fields, new services, or integrating ServiceNow should be easy.
- **Logging:** basic logs on ticket creation and status change.
- **Security (conceptual):** future support for SSO / OAuth2.

---

## 4. Architecture (High-Level)

The solution is structured using a **layered, service-oriented architecture**:

- **Presentation Layer:**  
  Web UI or API client.

- **API Layer:**  
  REST endpoints:
    - `POST /api/tickets`
    - `GET /api/tickets`
    - `GET /api/tickets/{id}`
    - `PATCH /api/tickets/{id}/status`

- **Service Layer:**
    - `TicketService` for business logic
    - `NotificationService` (future)
    - `UserService` (future)

- **Data Layer:**  
  Ticket entity + TicketRepository (H2)

- **Integration Layer:**  
  Optional connections to email providers, ServiceNow, identity providers.

For a deeper explanation see the diagrams in `/docs`.

---

## 5. Tech Stack

- Java 17
- Spring Boot
- Spring Web (REST)
- Spring Data JPA (H2)
- Maven
- (Optional) Angular or plain HTML/TS for the UI

---

## 6. How to Run (Backend Only)

> Final version updated once backend is implemented.

1. Clone:

```bash
git clone https://github.com/elenataleva/it-service-portal-architecture.git
cd it-service-portal-architecture/backend
