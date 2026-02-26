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
<img width="1531" height="632" alt="Architecture Diagram" src="https://github.com/user-attachments/assets/b1f57122-4a32-4362-a9be-fb06a569e26f" />

## Asset Identification & Security Objectives

The system's critical assets are labelled and mapped directly to the CIA Triad.

| Critical Asset | Description | Primary Security Objective(s) | Justification |
| :--- | :--- | :--- | :--- |
| **User Credentials & Tokens** | Passwords, session cookies, and authentication tokens. | Confidentiality | Unauthorized access leads to complete account compromise and system-wide impersonation. |
| **Academic Records** | Student grades, transcripts, and attendance logs. | Integrity & Confidentiality | Grades must be protected from unauthorized tampering; student privacy must be strictly upheld. |
| **Financial & Salary Data** | Student fee payments and faculty payroll information. | Integrity & Confidentiality | Requires transaction checks to prevent fraud and strict access controls to prevent data leaks. |
| **LMS Platform & Files** | Course materials, quizzes, and assignment uploads. | Availability | System disruptions prevent students from meeting critical academic deadlines and accessing essential study resources. |

## Threat Modeling

The threat model was created using the STRIDE methodology, risk was determined based on likelihood and impact. Full report is in Report.htm

| Threat Category | Threat Description & Affected Component | Impact | Risk Level |
| :--- | :--- | :--- | :--- |
| **Spoofing** | Pretending to be something or someone else. An attacker steals a session token over the Remote User Zone to bypass the Web Server and impersonate a faculty member. | Unauthorized access to courses and the ability to alter grades. | **High** |
| **Tampering** | Making unauthorized changes to something. A malicious user intercepts HTTP traffic and modifies the payload of an attendance submission to the Attendance Microservice. | Loss of data integrity; inaccurate academic records. | **High** |
| **Repudiation** | Claiming that you didn't do something or weren't responsible for an event. A student uploads an assignment to the LMS but later claims the system deleted it, and there are no logs to prove otherwise. | Lack of accountability; inability to enforce academic deadlines. | **Medium** |
| **Information Disclosure** | Giving information to someone who shouldn't see it. An insecure direct object reference (IDOR) on the Finance API allows a student to read another student's fee voucher. | Breach of privacy; potential compliance violations. | **High** |
| **Denial of Service** | Absorbing a services necessary resources. A botnet floods the Web Server during final exam week, exhausting resources and crashing the portal. | Loss of availability; severe disruption to university operations. | **High** |
| **Elevation of Privilege** | Allowing someone to do something that they're not authorized to do. A standard student user exploits an API misconfiguration to grant themselves Registrar/Admin permissions. | Complete system compromise; ability to modify any academic or financial record. | **High** |

