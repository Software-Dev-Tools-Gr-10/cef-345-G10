# Design Decision Records

## D-01: Use a browser client and layered application server

Date: 2026-11-21

Context: The confirmed SRS needs public event browsing and registration alongside protected event management, reporting and check-in actions (FR-01–FR-15). The group is building a course project with limited time.

Decision: Use a responsive browser client and one application server organized into routes, controllers, services and repositories.

Consequences: The group can deploy one application and keep business rules in services. The server can serve multiple screen sizes. The server remains a single point of availability, and local check-in requires a network connection as stated in the SRS.

## D-02: Use a relational data model with event-linked records

Date: 2026-11-21

Context: Tickets, registrations, payments and check-ins must remain connected to the correct event, and the report must count these records accurately (FR-05–FR-14).

Decision: Use relational tables with primary keys, foreign keys, uniqueness constraints and transactions for registration and check-in operations. The database engine is a group implementation choice.

Consequences: Referential integrity helps prevent orphan tickets and attendance records. The schema needs migrations if fields change. The chosen engine and hosting arrangement must be added once the group agrees on them.
