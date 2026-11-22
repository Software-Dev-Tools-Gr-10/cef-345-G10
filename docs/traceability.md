Traceability table

proposed by : **Tabe Miracle Fiagmenyi**
              **FE25A199**
based on previously written srs document

Tables assumed: `users`, `user_roles`, `events`, `registrations`, `tickets`, `payments`,
`check_ins`, `event_updates`.

| FR ID | Priority | Requirement (short) | Endpoint(s) | Table(s) |
|---|---|---|---|---|
| FR-01 | Must | Organizer creates draft event | POST /api/events | events, users |
| FR-02 | Must | Publish, update, close, cancel event | PATCH /api/events/:id, PATCH /api/events/:id/status | events |
| FR-03 | Must | Visitor sees event details, status, remaining capacity | GET /api/events, GET /api/events/:id | events, registrations |
| FR-04 | Must | Register one attendee (name + contact) | POST /api/events/:id/registrations | registrations |
| FR-05 | Must | Refuse unpublished, closed, expired, full, duplicate-contact | POST /api/events/:id/registrations (400, 409) | events, registrations |
| FR-06 | Must | Registration creates a unique ticket | POST /api/events/:id/registrations (201) | tickets, registrations, events |
| FR-07 | Must | Confirmation shows ticket ID, event, status | GET /api/tickets/:ticketId | tickets, events, registrations |
| FR-08 | Must | Organizer views/searches attendees | GET /api/events/:id/registrations?q= | registrations, tickets |
| FR-09 | Should | Record payment status, amount, method, date | PUT /api/registrations/:id/payment | payments, registrations |
| FR-10 | Must | Staff validate ticket ID for selected event | POST /api/events/:id/check-ins | tickets, events |
| FR-11 | Must | Reject nonexistent, wrong-event, used tickets with reason | POST /api/events/:id/check-ins (404, 409) | tickets, check_ins |
| FR-12 | Must | Record check-in time and staff account | POST /api/events/:id/check-ins (201) | check_ins, users |
| FR-13 | Should | Organizer posts updates to registered attendees | POST /api/events/:id/updates, GET /api/events/:id/updates | event_updates, events |
| FR-14 | Should | Attendance and payment reports, incl. past events | GET /api/events/:id/reports/attendance, GET /api/events/:id/reports/payments | check_ins, payments, registrations, events |
| FR-15 | Must | Admin assigns/removes Organizer and Check-in staff roles | PUT /api/users/:id/roles/:role, DELETE /api/users/:id/roles/:role | users, user_roles |
| FR-16 | Could | Export reports as CSV | GET /api/events/:id/reports/attendance?format=csv, GET /api/events/:id/reports/payments?format=csv | check_ins, payments, registrations |

Supporting endpoint: POST /api/auth/login (users, user_roles). It is needed by every
role-restricted FR (FR-01, 02, 08, 09, 10, 12, 13, 14, 15, 16) and by NFR-04.

**Gap check:** all 16 FRs map to at least one endpoint and one table, so there are no gaps.

**Non-functional hooks (for the design decisions):**
- NFR-02 (check-in within 2 s): index `tickets.id` and `check_ins.ticket_id`.
- NFR-04 and NFR-07: role check in the controller layer; hide contact fields in public GET /api/events/:id.
- FR-05 duplicate-contact rule: UNIQUE (event_id, contact) on `registrations`, still to be confirmed by the client.
