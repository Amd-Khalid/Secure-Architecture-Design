# Secure-Architecture-Design

# Secure Architecture & Threat Model: University Management System

## System Overview
This document contains the secure architecture design, threat model and risk management strategy for a University Management System. The architecture utilises Defense in Depth principles to secure data against external as well as insider threats.

The system utilises a microservice based architecture handling individual university operations.

**Core Components:**

- Web Frontend (Brower): The portal interface for all users (students, faculty) to be able to access.
- Web Server (API Gateway): The centralized entry point which handles routing, input validation and initial authentication checks.
- Backend Microservices: These are isolated services which handle specific requirements and logic
  - LMS Microservice (Coursework, assignments)
  - Attendance Microservice (Daily logging and tracking)
  - Finance Microservice (Fee processing and faculty payroll)
- Data Storage: An Azure SQL database for persistent data storage.

**Trust Boundaries**
In order to be able to accurately model threats, the architecture is divided with specific trust boundaries.

- Remote User Zone (Internet Boundary): The perimeter separating untrusted external browsers from the web server.
- Machine Trust Boundary (Internal Network): The segmentation which isolates the public web server from the internal backend microservices.
- Azure Trust Boundary (Data Isolation): The strict access boundary which separates application logic (microservices) from the data storage (Azure SQL Database)

**Architecture Diagram**
