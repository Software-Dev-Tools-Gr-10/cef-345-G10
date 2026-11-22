Design Decisions

Decision 1: QR Code Tickets

Status: Accepted

Context

The system needs a way to check tickets and confirm that people attending an event have registered. Checking names manually can take time, especially when many people are attending.

Decision

We decided to use QR codes for tickets. Each attendee will receive a ticket with a unique QR code which will be scanned during check-in.

Alternatives

- Checking attendees' names manually.
- Using ticket numbers.
- Using QR codes.

Reason

QR codes are easy to generate and scan. They also make the check-in process faster and help prevent people from using the same ticket more than once.

Consequences

- Check-in will be faster.
- Each ticket can be uniquely identified.
- Used tickets can be detected.
- A device is needed to scan the QR codes.

---

Decision 2: Three-Tier Architecture

Status: Accepted

Context

The system has different parts that handle the user interface, processing of information, and storage of data. Keeping these parts separate will make the system easier to manage.

Decision

We decided to use a three-tier architecture. It consists of the presentation layer, application layer, and data layer.

- Presentation Layer: This is where users interact with the system.
- Application Layer: Handles the main operations of the system, such as registration, ticket generation, and check-in.
- Data Layer: Stores information about events, users, tickets, and attendance.

Alternatives

- Combining all the system components into one structure.
- Separating the system into three layers.

Reason

The three-tier architecture makes the system easier to understand, maintain, and modify. It also allows different team members to work on different parts of the system.

Consequences

- Each layer has its own responsibility.
- Changes to one layer can be made without affecting the entire system.
- Communication between the layers is required.
- The system will be easier to expand in the future.
