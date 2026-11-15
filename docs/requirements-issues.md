# Week 4 GitHub issue drafts

These drafts map to the Must and Should requirements in `docs/srs.md`. Create one GitHub issue for each entry after the group has created the `CA submission` and `v1.0` milestones and the required labels. Assign each issue to a member. The Could requirement FR-16 is not included.

## FR-01 Create an event draft

- Label: `feature` · Milestone: `CA submission`
- User story: As an organizer, I want to enter an event's details so that I can prepare it for publication.
- Given an organizer enters all required values and a valid time range, when the organizer saves the event, then the system stores it as a draft.
- Given the end time is earlier than the start time or capacity is zero, when the organizer saves, then the system identifies the invalid field and does not publish the event.

## FR-02 Publish and manage an event

- Label: `feature` · Milestone: `CA submission`
- User story: As an organizer, I want to publish, update, close or cancel an event so that its registration status is clear.
- Given a valid draft, when the organizer publishes it, then visitors can see it and registration opens.
- Given an organizer closes or cancels an event, when an attendee attempts to register, then the system refuses the registration and shows the event status.

## FR-03 Browse published events

- Label: `feature` · Milestone: `CA submission`
- User story: As a visitor, I want to view published events and their availability so that I can decide whether to register.
- Given published and draft events exist, when a visitor opens the event list, then only published events appear.
- Given an event is full or closed, when a visitor views its details, then its registration status is shown.

## FR-04 Register one attendee

- Label: `feature` · Milestone: `CA submission`
- User story: As an attendee, I want to register with my name and contact details so that the organizer can record my attendance.
- Given registration is open and the attendee supplies a name and contact value, when the attendee submits the form, then one registration is saved.
- Given both contact fields are empty, when the attendee submits, then the system explains that at least one contact method is required and saves no registration.

## FR-05 Reject invalid and duplicate registrations

- Label: `feature` · Milestone: `CA submission`
- User story: As an organizer, I want the system to block registrations that cannot be accepted so that the attendee list stays accurate.
- Given an event is full, closed, unpublished or past its deadline, when an attendee submits a registration, then the system refuses it and explains why.
- Given a registration for that event already has the same contact value after leading and trailing spaces are removed and letters are converted to lowercase, when another registration is submitted, then the system refuses the duplicate and creates no second ticket.

## FR-06 Generate a unique ticket

- Label: `feature` · Milestone: `CA submission`
- User story: As an attendee, I want a unique ticket after registration so that staff can verify my entry.
- Given a registration is accepted, when the system creates a ticket, then it assigns a non-empty identifier that is unique across tickets.
- Given two attendees register for the same event, when both registrations complete, then their ticket identifiers differ and each ticket points to the correct attendee and event.

## FR-07 View a ticket

- Label: `feature` · Milestone: `CA submission`
- User story: As an attendee, I want to view my ticket and event details so that I can present it at the entrance.
- Given a registration succeeds, when the confirmation page opens, then it shows the ticket identifier, event details and ticket status.
- Given no ticket was created for a failed registration, when the confirmation page is requested, then the system does not display a valid ticket.

## FR-08 View and search registrations

- Label: `feature` · Milestone: `CA submission`
- User story: As an organizer, I want to view and search an event's attendee list so that I can find a registration quickly.
- Given an organizer opens an event's attendee list, when registrations exist, then the list shows each attendee and registration status.
- Given the organizer searches by an attendee name or contact value, when matching records exist, then only matching registrations are returned.

## FR-09 Record ticket payment

- Label: `feature` · Milestone: `v1.0`
- User story: As an organizer, I want to record the payment status and details for a registration so that I can reconcile ticket payments.
- Given an authorized organizer records a payment, when the status, amount, method and date are valid, then the registration shows the saved payment details.
- Given a user without the Organizer role attempts to change a payment record, when the request is submitted, then the system denies it and leaves the payment record unchanged.

## FR-10 Validate a ticket

- Label: `feature` · Milestone: `CA submission`
- User story: As check-in staff, I want to validate a ticket for the selected event so that only registered attendees are admitted.
- Given a valid unused ticket belongs to the selected event, when staff submit its identifier, then the system returns a valid result and the attendee's name.
- Given an unknown ticket identifier is submitted, when validation runs, then the system returns an invalid result and does not create a check-in.

## FR-11 Reject wrong-event and reused tickets

- Label: `feature` · Milestone: `CA submission`
- User story: As check-in staff, I want the system to reject tickets for another event or tickets already used so that tickets cannot be reused.
- Given a ticket belongs to a different event, when it is submitted for the selected event, then the system rejects it and explains the mismatch.
- Given a ticket already has a check-in record, when it is submitted again, then the system rejects the second check-in and displays the prior status.

## FR-12 Record a successful check-in

- Label: `feature` · Milestone: `CA submission`
- User story: As an organizer, I want every successful check-in recorded with its time and staff member so that the attendance record is traceable.
- Given a valid unused ticket is accepted by an authorized staff member, when check-in completes, then one record stores the ticket, event, time and staff account.
- Given check-in completes, when the organizer views the event report, then the attendee is counted once as checked in.

## FR-13 Publish an event update

- Label: `feature` · Milestone: `v1.0`
- User story: As an organizer, I want to publish an event update for registered attendees so that they can see important changes.
- Given an organizer posts an update for an event, when a registered attendee views that event, then the update is visible with its publication time.
- Given a user is not an organizer for the event, when that user attempts to publish an update, then the system denies the action.

## FR-14 View event totals

- Label: `feature` · Milestone: `v1.0`
- User story: As an organizer, I want registration, payment and check-in totals for an event so that I can review attendance and collections.
- Given an organizer opens an event report, when registrations, payments and check-ins exist, then the report shows registration count, paid-registration count, total amount recorded as paid and checked-in count.
- Given a known set of registrations and payment records exists, when the report loads, then its counts and collected amount match those records.

## FR-15 Assign user roles

- Label: `feature` · Milestone: `CA submission`
- User story: As an organizer, I want to assign Organizer and Check-in staff roles so that each user can perform only their event duties.
- Given an authorized organizer assigns the Check-in staff role to a user, when the assignment is saved, then that user can access ticket validation for the assigned event.
- Given a user has no Check-in staff or Organizer role, when the user attempts a check-in, then the system denies the action.

## NFR-01 Event-page performance

- Label: `Test` · Milestone: `CA submission`
- User story: As an attendee, I want event pages to load quickly so that I can register without delay.
- Given a load test with up to 50 concurrent users, when 100 event-list and event-detail requests run, then at least 95% complete within 3 seconds.
- Given the load test completes, when results are reviewed, then the report records concurrency and response times for the requests.

## NFR-02 Check-in response time

- Label: `Test` · Milestone: `CA submission`
- User story: As check-in staff, I want ticket validation to respond quickly so that attendees do not wait in a long queue.
- Given up to 20 concurrent check-in requests, when 100 validations run, then at least 95% return within 2 seconds.
- Given a validation exceeds 2 seconds, when the test report is produced, then the slow response is counted and reported.

## NFR-03 Mobile layout

- Label: `Test` · Milestone: `CA submission`
- User story: As an attendee or check-in staff member, I want the system to work on a narrow phone screen so that I can use it at the venue.
- Given a 360 CSS-pixel viewport, when the registration, ticket and check-in screens open, then no horizontal page scrolling is required.
- Given the viewport remains 360 CSS pixels, when the user completes registration or check-in, then all required controls remain visible and operable.

## NFR-04 Role-based security

- Label: `Test` · Milestone: `CA submission`
- User story: As an organizer, I want restricted actions protected by role checks so that attendee and event data cannot be changed by unauthorized users.
- Given an attendee account requests an organizer-only action, when the request reaches the system, then the system denies it and leaves data unchanged.
- Given a check-in staff account requests payment editing or reporting, when the request is processed, then the system denies access.

## NFR-05 Check-in usability

- Label: `Test` · Milestone: `v1.0`
- User story: As check-in staff, I want the check-in flow to be quick and clear so that I can process attendees at the entrance.
- Given five trained staff members test valid tickets on a phone-sized screen, when each completes the check-in flow, then at least four finish within 10 seconds.
- Given the same test, when staff submit an invalid or previously used ticket, then the result clearly states why it was rejected.

## NFR-06 Check-in availability

- Label: `Test` · Milestone: `v1.0`
- User story: As an organizer, I want the check-in service available during the event so that staff can verify tickets.
- Given a published four-hour check-in window, when service availability is monitored, then the service is available for at least 99% of that window.
- Given the service becomes unavailable during the window, when the incident is reviewed, then its duration is included in the availability calculation.

## NFR-07 Attendee privacy

- Label: `Test` · Milestone: `v1.0`
- User story: As an attendee, I want my contact details hidden from public visitors and unnecessary staff roles so that my information is protected.
- Given a public visitor opens an event page, when the page loads, then no attendee contact details are displayed.
- Given a check-in staff account views a ticket, when attendee details appear, then contact details and payment information are hidden.

