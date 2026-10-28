# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved), `inputs/02-current-meeting.md`
(discovery notes, current). `inputs/03-superseded-request.md` is superseded history and
approves nothing in this intent.

## Problem

- **I-1** The clinic receives avoidable calls from patients checking whether their results
  are ready. These status-only calls take support time without giving the patient anything
  they could not see themselves once the result is released.
  > "The fictional clinic receives avoidable calls from patients checking whether results are ready." (PRD)

## Proposed outcome

- **I-2** The patient is told that an update is available once a result is ready for their
  review, and is directed to the existing authenticated portal to see it.
  > "Notify the patient when a result is ready for their review in the existing authenticated portal." (PRD)
- **I-3** "Ready" means a doctor has reviewed **and** released the result. A finished
  laboratory upload alone never makes a result eligible for notification.
  > "A doctor must review and release the result before any notification is eligible." (PRD)
  > "'Ready' must not mean that the laboratory upload merely finished." (Product owner, discovery notes)

### How success will be observed

- **I-4** Results-status calls, counted separately from clinical questions and compared
  against a baseline measured during discovery, go down.
  > "Count status-only calls separately from clinical questions; compare against a measured baseline." (Support lead)
- **I-5** Notification delivery failures are measured and tracked.
  > "Measure reduction in results-status calls and delivery failures." (PRD)
- **I-6** No numeric target is approved. The baseline and target will be agreed during
  discovery; until then success is observed as a direction of change against the measured
  baseline, not as a fixed number.
  > "The baseline and numerical target will be agreed during discovery; none is approved yet." (PRD)

## Affected users and systems

- **I-7** Patients: the only approved recipients of the notification.
- **I-8** Doctors: review and release results; release is the trigger for eligibility.
- **I-9** Support staff: handle the calls this aims to reduce and classify status-only calls
  versus clinical questions for measurement.
- **I-10** Existing authenticated patient portal and its existing result route: where the
  patient views the result.
  > "The existing portal path is adequate." (Engineering lead)
- **I-11** Results release workflow (doctor review and release status) and the laboratory
  results feed, which on its own is not a trigger.
- **I-12** Notification sending and its operational logs, including duplicate prevention
  and delivery-failure handling.

## Out of scope

- **I-13** Portal redesign. Excluded by the PRD.
- **I-14** A new or rebuilt patient dashboard. "A new patient dashboard is a separate project." (Engineering lead)
- **I-15** Appointment scheduling. Excluded by the PRD.
- **I-16** Clinical interpretation of results. Excluded by the PRD.
- **I-17** Notifying third parties. Excluded by the PRD. The referring doctor is not approved
  as a recipient; that decision stays open (OPEN-2).
- **I-18** Everything requested only by the superseded early request is excluded: sending
  results automatically on lab upload, putting result content in a message, copying the
  referring doctor, rebuilding the dashboard and its call-reduction target.
  > "Status: SUPERSEDED by approved PRD N-17 ... This is historical context, not current approval." (Early request)

## Constraints

- **C-1** Review before notification: no notification is eligible until a doctor has
  reviewed and released the result.
- **C-2** Message content: the notification only says that an update is available and points
  to the existing authenticated portal. No diagnosis, test name or result, clinical
  measurement or treatment detail in the message or in notification and operational logs.
  > "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs." (PRD)
  > "No sensitive content in messages or operational logs." (Privacy owner)
- **C-3** Access: results are viewed only after authentication in the existing portal. No
  attached results and no public result link.
- **C-4** Duplicates: at most one notification per released result version; delivery
  failures are handled without sending duplicate patient messages.
- **C-5** Route: use the existing portal result route; no new portal pages or dashboard.
- **C-6** Recipients: only the patient. No other recipient is added until the owners of
  OPEN-2 decide.

## Open questions and owners

- **OPEN-1 Channel.** Which channel carries the notification: SMS, email or portal push?
  **Owners: product owner and privacy owner.** Decided after the consent and delivery
  review. Not decided in this intent.
  > "Channel (SMS, email, portal push): product owner and privacy owner must decide after consent/delivery review." (PRD)
- **OPEN-2 Referring-doctor notification.** May the referring doctor be notified, and with
  what recipient authorization? **Owners: clinical owner and privacy owner.** Not approved;
  approval must not be inferred from the superseded request.
  > "Clinical owner and privacy owner must decide recipient authorization. It is not approved by this PRD." (PRD)
  > "Do not infer approval for referring-doctor notifications from the old request." (Clinical owner)

The measured baseline and any numeric target are agreed during discovery (see I-6); this
intent does not set them.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
