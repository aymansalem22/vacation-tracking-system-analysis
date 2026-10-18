# Vacation Tracking System — Analysis & Design

Software engineering analysis and design practice for a Vacation Tracking System.

## 1. Vision

A Vacation Tracking System (VTS) aims to empower employees, improve vacation management, and reduce administrative workload.

 

## 2. Functional Requirements

- **FR-01:**: The system must validate and verify leave requests using rules.
- **FR-02:**: The system must support manager approval when required.
- **FR-03:**: The system must allow users to view vacation requests from the previous calendar year.
- **FR-04:**: The system must allow vacation requests up to 18 months in the future.
- **FR-05:**: The system must send emails to request manager approval and notify employees of status changes.
- **FR-06:**: The system must record activity logs for all transactions.
- **FR-07:**: The system must allow HR staff and system administrators to override actions blocked by rules and log these overrides.
- **FR-08:**: The system must allow managers to award personal leave time within system-set limits.
- **FR-09:**: The system must provide a web service for other internal systems to get employee vacation request summaries.
- **FR-10:**: The system must get required employee information and changes from existing HR systems.

## 3. Non-Functional Requirements

- **NFR-01 (Usability):** The system shall be easy to use and intuitive for employees.

## 4. Constraints

- **CON-01 (Existing Infrastructure):** The system shall use the existing hardware and middleware.
- **CON-02 (Intranet Portal):** The system shall be implemented as an extension to the existing intranet portal system.
- **CON-03 (Authentication):** The system shall use the existing intranet portal's Single Sign-On (SSO) mechanism for all authentication.

## 5. Domain — Define Problem

### 5.1 Business Problem

The vacation request process is slow because it involves multiple manual steps. Employees need an easier way to manage their vacation time without complicated procedures.

### 5.2 Domain Concepts

- Employee
- Manager
- Human Resources (HR)
- Vacation Request
- Leave Policy
- Vacation Balance

### 5.3 System Boundaries

**Inside the System:**
- Validate and verify leave requests using business rules.
- Send email notifications for manager approval and request status changes.
- Allow authorized HR staff and system administrators to override restricted actions and log these overrides.

**Outside the System:**
- Choosing vacation dates on behalf of employees. *(Inferred from the employee's responsibility to manage their own leave.)*

**External Systems:**
- Existing HR legacy systems used to retrieve employee information and updates.

## 6. Actors

### Employee
The employee uses the system to manage his or her vacation time.

### Manager
The manager has the same capabilities as an employee. The manager also approves vacation requests from direct subordinates and can award comp time within system-set limits.

### Clerk (HR)
The HR clerk views employee personal data, keeps employee information accurate and up to date across HR systems, and can add or remove most records in the VTS.

### System Admin
The system administrator manages the system's technical resources, such as the web server and database, and collects and archives log files.


## 7. Manage Time — Use Case Analysis

### 7.1 Entities (Data Model)

![Manage Time Data Model](diagrams/managetime_datamodel.jpg)

### 7.2 Flowchart

![Manage Time Flowchart](diagrams/managetime_flowchart.png)

### 7.3 Sequence Diagram

![Manage Time Sequence Diagram](diagrams/managetime_sequencydiagram.png)


### 7.4 Pseudocode
BEGIN ManageTime

    REQUIRE employee is authenticated and authorized

    Employee opens VTS
    Display vacation requests and leave balances

    Employee selects a leave category with positive balance
    Display calendar and request form

    REPEAT
        Employee enters dates, hours, title, description
        Employee submits request
        Validate request

        IF request is invalid THEN
            Display validation errors
            Ask employee to edit or cancel

            IF employee cancels THEN
                END ManageTime
            END IF
        END IF
    UNTIL request is valid

    Return employee to VTS home

    IF manager approval is required THEN
        Send approval email to authorized manager
        Set request status to PENDING_APPROVAL

        Manager opens VTS
        Authenticate manager if required
        Display pending requests
        Manager selects and reviews request

        IF manager approves THEN
            Set request status to APPROVED
        ELSE
            Require rejection explanation
            Set request status to REJECTED
        END IF

        Send result email to employee
        Return manager to VTS home
    ELSE
        // Further processing not specified in main flow
    END IF

END ManageTime
