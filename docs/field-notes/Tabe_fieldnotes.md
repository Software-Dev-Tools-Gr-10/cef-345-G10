# Event registration and check-in field notes

- interviewer: Tabe Miracle Fiagmenyi
- Matricule: FE25A199

## Visit details

- Organisation or venue: Small private events company 
- Person's role in organization: Event organiser
- Permission to take notes confirmed: Yes

## Questions

1. Please walk me through the most recent event you organised, from planning until people arrived.
2. How did people register, and where did you keep the list?
3. Tell me about the last time the expected attendance differed from who arrived.
4. How did you check tickets or names at the entrance? What happened with a duplicate or unclear entry?
5. How did you record payments and confirm the amount collected?
6. How did you tell registered people about a time or venue change?
7. What report or attendance information did you need after the event?
8. What is the most difficult or time-consuming part of this process?

## Observations

| Time | What the organiser did or said | Record, tool or information used | Problem, exception or workaround | Evidence  |
|---|---|---|---|---|
| 0:05 | Said guests register by phone call or WhatsApp message and she writes each name in a notebook | Paper notebook | Names are sometimes misspelt or written twice | Reported |
| 0:20 | Showed the sales list; payments are mostly mobile money, confirmed by checking her phone messages | Phone messages, notebook | She matches each payment to a name by hand; some payments cannot be matched to a person | Observed |
| 0:35 | Described the last event: about 120 expected, about 150 arrived | Printed guest list | Extra people were added by hand at the gate, which slowed the queue | Reported |
| 0:45 | Said two helpers tick names on a printed list at the entrance | Printed list, pens | Two people shared one ticket and nobody could tell who had entered first | Reported |
| 0:55 | Said she sends venue changes by individual WhatsApp messages and some guests miss them | WhatsApp | No record of who was told | Reported |

## Requirements implications

| Observed need or rule | Who needs it | Possible requirement ID | Question to confirm |
|---|---|---|---|
| Register once, with no duplicate or misspelt names | Attendee, Organizer | FR-xx (registration and account) | Is a phone number or email the main identifier? |
| Payment automatically linked to the right ticket | Organizer, Attendee | FR-xx (purchase and payment) | Which payment methods must be supported? |
| Each ticket usable only once at the entrance | Check-in staff | FR-xx (duplicate detection) | What should staff do when a ticket is already used? |
| Fast check-in with a way to add late guests | Check-in staff | NFR-xx (check-in speed), FR-xx (manual check-in) | Are walk-ins allowed, and how are they paid? |
| Message all registered guests about a change | Organizer, Attendee | FR-xx (notifications) | Is SMS, WhatsApp or email preferred? |
| Attendance and sales summary after the event | Organizer | FR-xx (reports) | Which figures does she need in the report? |

## Unanswered questions

- How many events does she organise in a year?
- What does she do about refunds?
- What internet access is available at the entrance?

## Follow-up

- Details the client should confirm: payment methods, how walk-ins are handled, preferred notification channel
- Requirements that should change after this visit: add duplicate-ticket detection and late-guest entry to the Must and Should lists if not already there
