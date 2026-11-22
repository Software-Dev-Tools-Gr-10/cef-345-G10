proposed by : **Tabe Miracle Fiagmenyi**
                **FE25A199**

**Use case.** At the gate, check-in staff scan a ticket's QR code (or type the ticket ID) on the check-in screen for the selected event. The system confirms the user is check-in staff, the ticket exists, belongs to this event and has not been used, then records the check-in time and the staff account (FR-10 to FR-12).

**Sequence diagram:** checkin-sequence.drawio.svg



**sequenceDiagram :**

    actor Staff as Check-in staff
    participant B as Browser
    participant C as CheckInController
    participant S as CheckInService
    participant T as TicketRepository
    participant K as CheckInRepository
    participant L as AuditRepository
    participant D as Database

    Staff->>B: Scans QR code, taps Check in
    B->>C: POST /api/events/{eventId}/check-ins { ticketId }
    C->>C: authenticate: session user and role
    opt user is not Check-in staff
        C->>S: recordDeniedAttempt(userId, eventId)
        S->>L: insert(userId, eventId, now)
        L->>D: INSERT INTO denied_attempts
        D-->>L: ok
        L-->>S: ok
        S-->>C: ok
        C-->>B: 403 "You cannot check in attendees"
        B-->>Staff: Red screen with the message
    end
    C->>C: validate body (else 400)
    C->>S: checkIn(eventId, ticketId, staffId)
    S->>D: BEGIN TRANSACTION
    S->>T: findById(ticketId)
    T->>D: SELECT ticket WHERE id = ? FOR UPDATE
    D-->>T: ticket row or nothing
    T-->>S: ticket or null
    alt ticket not found
        S->>D: ROLLBACK
        S-->>C: throw TicketNotFoundError
        C-->>B: 404 "Ticket not found"
        B-->>Staff: Red screen with the message
    else ticket belongs to another event
        S->>D: ROLLBACK
        S-->>C: throw WrongEventError
        C-->>B: 422 "Ticket is for another event"
        B-->>Staff: Red screen with the message
    else ticket already used
        S->>D: ROLLBACK
        S-->>C: throw AlreadyCheckedInError(checkedInAt)
        C-->>B: 409 "Already checked in at 18:42"
        B-->>Staff: Amber screen with the message
    else valid ticket, not yet used
        S->>K: insert(ticketId, staffId, now)
        K->>D: INSERT INTO check_ins
        D-->>K: checkInId
        K-->>S: checkIn record
        S->>T: markUsed(ticketId)
        T->>D: UPDATE tickets SET status = 'used'
        D-->>T: ok
        T-->>S: ok
        S->>D: COMMIT
        S-->>C: attendeeName, checkedInAt
        C-->>B: 201 { attendeeName, checkedInAt }
        B-->>Staff: Green screen "Welcome, name"
    end


**Steps**

1. The browser sends `POST /api/events/{eventId}/check-ins` with the ticket ID. The event is the one staff selected on the screen.
2. `CheckInController` reads the session user and role. If the user is not check-in staff, the attempt is recorded through the service and the controller returns 403 (NFR-04).
3. The controller checks the body is well formed. If not: 400.
4. The controller calls `CheckInService.checkIn(eventId, ticketId, staffId)`.
5. The service opens a transaction and asks `TicketRepository` for the ticket, locking its row.
6. **alt not found / wrong event / already used:** the service rolls back and throws a typed error; the controller turns it into the status and message below. The ticket is left unchanged.
7. **alt valid:** the service records the check-in (time and staff account), marks the ticket used, commits, and the controller returns 201 with only the attendee's name and the check-in time (NFR-07).

**Error cases**

| Case | Raised by | Status | Message shown to staff |
|---|---|---|---|
| User is not check-in staff | controller (attempt recorded via service) | 403 | "You cannot check in attendees" |
| Body missing or malformed | controller | 400 | "Enter or scan a ticket ID" |
| No ticket with that ID | `TicketNotFoundError` | 404 | "Ticket not found" |
| Ticket is for a different event | `WrongEventError` | 422 | "Ticket is for another event" |
| Ticket already used | `AlreadyCheckedInError` | 409 | "Already checked in at 18:42" (time of the first check-in) |
| Unexpected failure | any other error | 500 | "Something went wrong, try again" |

**Design decisions**

- **All checks happen before anything is written.** The three failure branches roll back with nothing to undo, and the ticket stays unchanged, as UC-03 requires.
- **The two writes live in one transaction.** Inserting the `check_ins` row and setting `tickets.status = 'used'` must both happen or neither; otherwise a ticket could be marked used with no check-in record, or the reverse. This keeps the rule "a valid ticket gets exactly one check-in record".
- **`FOR UPDATE` on the ticket row.** If two gates scan the same ticket at the same moment, the second waits, then sees `status = used` and gets the 409. Row-level locking does not slow other tickets, so NFR-02 (20 concurrent check-ins within 2 s) is not affected.
- **The service owns the transaction and the business rules** (wrong event, already used). The controller handles HTTP and the role check; each repository runs SQL for its own table.
- **Minimal data returned.** The response has no contact details, only what staff need to identify the ticket holder (NFR-07).
