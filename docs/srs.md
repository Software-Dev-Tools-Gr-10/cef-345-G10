# Software Requirements Specification

## Event Ticketing and Check-in System

Prepared by: Luma Chrisparr Ngale Jomia  
Matricule: FE25A114  
Role: Analysis Lead  
Status: Working draft for client and group review

## 1. Introduction

### Purpose

This specification describes the requirements for an Event Ticketing and Check-in System for a church or student association that organises events. It gives the group a shared description of the system to design, build and test.

### Scope

The system will support event publishing, attendee registration, ticket creation and validation, check-in, payment-status records, event updates and attendance reporting. It is intended to provide organisers with one reliable event registration record and to reduce ticket reuse and inaccurate attendance counts.

### Definitions

| Term | Meaning |
|---|---|
| Attendee | A person who registers for an event. |
| Organizer | An authorised user who creates and manages events. |
| Check-in staff | An authorised user who validates tickets at an event entrance. |
| Ticket | A unique record linked to one attendee registration and one event. |
| Check-in | Recording that an attendee with a valid ticket has arrived. |

## 2. Overall description

### Product perspective

The product is a web application for event organisers, attendees and check-in staff. Attendees use it to view events, register and access tickets. Organisers manage events and registrations. Check-in staff validate tickets and record attendance.

### Stakeholders and users

| Stakeholder or user | Main interest |
|---|---|
| Church or student association leadership | Accurate event registration, payment and attendance information. |
| Event organizer | Create events, set a ticket price or mark an event free, manage attendees, record payments, communicate changes and review reports. |
| Attendee | See whether an event is free or paid and its price before registering, then access a valid ticket. |
| Check-in staff | Verify tickets quickly and avoid duplicate entry. |
| Development group | Build and maintain the system within the course project constraints. |

### Operating environment

The system will be accessed through a web browser on a computer or phone. The expected venue workflow requires an internet connection for online ticket validation. Offline check-in is not included in this draft.

### Constraints

- The system must support a phone browser with a viewport as narrow as 360 CSS pixels.
- Event access and check-in actions must be restricted by user role.
- Each successful registration must have one unique ticket for one event.
- The course implementation technology and deployment host remain to be confirmed by the group.

### Assumptions and questions to confirm

- The assigned client may be a church or a student association; the specific organisation and its current process have not yet been named in this draft.
- Payment tracking means an organizer records payment status manually. Online payment processing is not assumed.
- The draft allows an event to be free (price set to zero) or paid (positive price). The client must confirm whether both options are used and whether payment is required before a ticket is issued or accepted at check-in.
- A ticket is displayed on a confirmation page after registration. Email or SMS delivery is not assumed until the client selects a delivery channel.
- Each registration is for one attendee. Group registration and ticket transfers are excluded unless the client requests them.
- The client must confirm whether a trimmed, case-insensitive contact value is a suitable duplicate-registration rule, which payment states are needed, and who can publish event changes.
- The response-time, concurrent-user, usability and availability values are proposed targets; the group must confirm they fit the client and the available hosting and test setup.
- The group should validate these assumptions using field notes and a discussion with an event organiser before approving the SRS.

## 3. Functional requirements

| ID | Priority | Requirement |
|---|---|---|
| FR-01 | Must | The system shall let an organizer create a draft event with a title, description, venue, start and end time, capacity, registration deadline and ticket price. |
| FR-02 | Must | The system shall let an organizer publish, update, close or cancel an event. Only published events with valid required details shall accept registrations. |
| FR-03 | Must | The system shall let a visitor view published event details, registration status and remaining capacity. |
| FR-04 | Must | The system shall let an attendee register one person for an open event by providing a name and at least one contact method. |
| FR-05 | Must | The system shall reject a registration if the event is unpublished, closed, past its deadline, at capacity, or already has a registration for that event with the same contact value after leading and trailing spaces are removed and letters are converted to lowercase. |
| FR-06 | Must | After a registration succeeds, the system shall create a ticket with a unique ticket identifier linked to that attendee and event. |
| FR-07 | Must | The system shall let an attendee view the ticket identifier, event details and ticket status on a confirmation page. |
| FR-08 | Must | The system shall let an organizer view and search an event's attendee list by attendee name or contact value. |
| FR-09 | Should | The system shall let an authorized organizer record whether a registration is unpaid or paid, the amount received, payment method and date recorded. |
| FR-10 | Must | The system shall let authorized check-in staff validate a ticket identifier against the selected event. |
| FR-11 | Must | The system shall reject a ticket that does not exist, belongs to another event, or has already been checked in, and shall show the reason. |
| FR-12 | Must | After a valid ticket is accepted, the system shall record the check-in time and the staff account that performed the check-in. |
| FR-13 | Should | The system shall let an organizer publish an event update that is visible to attendees registered for that event. |
| FR-14 | Should | The system shall show an organizer, for a selected event, the registration count, paid-registration count, total amount recorded as paid and checked-in count. |
| FR-15 | Must | The system shall let an authorized organizer assign and remove the Organizer and Check-in staff roles for user accounts. |
| FR-16 | Could | The system may let an organizer export an event's attendee and attendance report as a CSV file. |

## 4. Non-functional requirements

| ID | Priority | Requirement |
|---|---|---|
| NFR-01 | Must | With up to 50 concurrent users, 95% of event-list and event-detail requests shall complete within 3 seconds, measured in the group's agreed test environment. |
| NFR-02 | Must | With up to 20 concurrent check-in requests, 95% of ticket validations shall return a result within 2 seconds, measured in the group's agreed test environment. |
| NFR-03 | Must | Attendee registration, ticket display and check-in screens shall be usable at a 360 CSS-pixel viewport width without horizontal scrolling. |
| NFR-04 | Must | A user without the required role shall be denied event-management, payment-recording, reporting and check-in actions; automated tests shall verify each denied action. |
| NFR-05 | Should | In a usability test with five trained check-in staff, at least four shall complete a valid ticket check-in in 10 seconds or less using a phone-sized screen. |
| NFR-06 | Should | During a published event's scheduled check-in window, the deployed service shall be available at least 99% of the time, measured over that window. |
| NFR-07 | Should | Contact details shall not be visible to public event visitors; check-in staff shall see only the attendee information needed to identify the ticket holder. |

## 5. Use case diagram and descriptions

### Use case diagram

The editable UML use case diagram is at `docs/diagrams/usecases.drawio.svg`. It shows the actors Attendee, Organizer and Check-in staff, the system boundary and the main use cases. The designer should review it against the client process before the group approves the SRS.

### UC-01 Create and publish an event

| Field | Description |
|---|---|
| Primary actor | Organizer |
| Preconditions | The organizer is signed in and has the Organizer role. |
| Trigger | The organizer chooses to create an event. |
| Main flow | 1. The organizer enters the event title, description, venue, start and end time, capacity, registration deadline and ticket price. 2. The system checks that required values are present, the end time follows the start time, capacity is positive and ticket price is not negative. 3. The organizer saves the event as a draft. 4. The organizer chooses Publish. 5. The system marks the event as published and makes it visible to visitors. |
| Alternative flows | 2a. One or more values are invalid: the system identifies the fields to correct and does not save the invalid event. 4a. Required details are missing: the system keeps the event as a draft and explains what is missing. |
| Postconditions | A valid event is saved. If published, it is visible to visitors and can accept registrations until closed, full or past its registration deadline. |
| Requirements covered | FR-01, FR-02, FR-03 |

### UC-02 Register for an event and receive a ticket

| Field | Description |
|---|---|
| Primary actor | Attendee |
| Preconditions | The event is published, registration is open and capacity remains. |
| Trigger | The attendee chooses Register for an event. |
| Main flow | 1. The system displays the event details and registration form. 2. The attendee enters their name and at least one contact method. 3. The attendee submits the form. 4. The system checks event status, deadline, capacity and duplicate contact information. 5. The system saves the registration and creates a unique ticket identifier. 6. The system displays a confirmation page with the ticket identifier, event details and ticket status. |
| Alternative flows | 4a. The event is closed, full or past the deadline: the system refuses registration and explains why. 4b. The contact value already has a registration for the event: the system refuses the duplicate and explains how to find the existing ticket. 5a. Registration cannot be saved: the system does not issue a ticket and displays an error. |
| Postconditions | On success, one attendee registration and one unique ticket exist for the event. On failure, no ticket is issued. |
| Requirements covered | FR-03, FR-04, FR-05, FR-06, FR-07 |

### UC-03 Validate a ticket and check in an attendee

| Field | Description |
|---|---|
| Primary actor | Check-in staff |
| Preconditions | The staff member is signed in with the Check-in staff role, and the event is selected. |
| Trigger | The staff member scans or enters a ticket identifier. |
| Main flow | 1. The staff member submits the ticket identifier. 2. The system checks that the ticket exists and belongs to the selected event. 3. The system checks that the ticket has not already been used. 4. The system shows a valid result and the minimum attendee details needed for identification. 5. The system records the check-in time and staff account. 6. The system shows confirmation. |
| Alternative flows | 2a. The ticket does not exist or belongs to another event: the system rejects it and explains the reason. 3a. The ticket was already used: the system rejects a second check-in and shows the previous check-in status. 1a. The staff member is not authorized: the system denies the action and records the failed attempt. |
| Postconditions | A valid ticket has one check-in record. Invalid or previously used tickets remain unchanged. |
| Requirements covered | FR-10, FR-11, FR-12, NFR-02, NFR-04, NFR-07 |

## 6. Priorities

| Priority | Requirements | Reason |
|---|---|---|
| Must have | FR-01–FR-08, FR-10–FR-12, FR-15, NFR-01–NFR-04 | The system cannot manage event registrations and prevent unreliable or duplicate check-ins without these requirements. |
| Should have | FR-09, FR-13, FR-14, NFR-05–NFR-07 | Payment records, attendee updates, reports and additional quality targets address important client problems, with manual workarounds available. |
| Could have | FR-16 | CSV export is useful but can be deferred if the organizer can review the on-screen report. |
| Won't have this time | Online payment processing, SMS/WhatsApp delivery, ticket transfers and offline check-in | These items need additional services or rules not established in the current project brief. |

Every Must and Should requirement needs a GitHub issue with a user story, at least two Given/When/Then acceptance criteria, an appropriate label and a CA submission or v1.0 milestone.

## 7. Out of scope

- Processing card or mobile-money payments inside the system. The first version records payment status entered by an organizer.
- Sending SMS or WhatsApp messages. The first version displays event updates in the system.
- Transferring, reselling or refunding tickets.
- Offline ticket validation when the venue has no internet connection.
- Seat selection, transport booking, accommodation and event programme management.

