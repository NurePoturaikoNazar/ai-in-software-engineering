# Mini Issue Tracker — approved low-fidelity design brief v1.0

## Purpose and scope

Internal desktop web issue tracker for one small team. Include US-01–US-08 only: create, view, assign, prioritize, track status, comment and view important non-closed issues. Release scope comes from S1 §8, not Candidate MVP. Deferred: US-09. Future: SR-10 and SR-11. The source has no US-10 or US-11. Use three fictional users: Member A, Member B, Lead. Member A and Member B are Team Members; Lead is Team Lead. Assignee is an issue-specific relationship, not a separate system role.

## Design and output constraints

Create editable native Figma layers on one page. Grayscale, low-fidelity, readable hierarchy, meaningful layer names, reusable local controls and rows, Auto Layout where practical. Preserve Screen IDs in frame names. Reviewer notes, actor labels and source references stay OUTSIDE product frames in REF-01. No decorative page icons or empty square placeholders. No row-selection checkboxes, no bulk actions, no branding or decorative imagery.

Create canonical frames: S-01, S-02-A, S-02-B, S-02-C, M-01, S-03. Additional frames allowed for state examples (validation, save-failure, prepared transitions). REF-01 is a service section with permission matrix, transition matrix and state examples placed OUTSIDE product frames. Do not create 16 complete detail screens for role × status combinations.

Prototype navigation is annotated only; do not wire interactions or implement an application. Do not export files, implement backend, authentication, conditional logic or server-side permission checks in code.

## S-01 — Authentication boundary [BR-01; D-02]

A neutral boundary block with the application name and the exact label “Authentication entry — mechanism TBD”. No login method, password fields, SSO buttons or social-login controls. This is not working authentication. The demo starts from S-02 as an already authenticated user.

## S-02 — Issues List [US-02; US-05; US-08; D-01]

Header: title “Issues List” and Create Issue action C-03. Tabs: All Issues C-01 / Important Issues C-02. Rows C-04 show ID, Title, Status, Priority and Assignee if assigned. Unassigned rows show the empty-value label “Unassigned”. Title opens the corresponding issue details. No row-selection checkboxes.

Show three states of ONE product screen:
- S-02-A All Issues — all eight baseline rows below.
- S-02-B Important Issues — only ISS-001, ISS-002, ISS-003 and ISS-007.
- S-02-C Empty All Issues — zero registered issues, label “No issues yet”, tabs and Create Issue still visible.

These states may use separate frames for demonstration.

Important = (priority is High or Critical) AND status != Closed. Resolved High/Critical issues stay included. Low, Normal, unset priority and every Closed issue are excluded. Reuse the same issue IDs and values across list states.

### Baseline sample data

| ID | Title | Status | Priority | Assignee |
|---|---|---|---|---|
| ISS-001 | Login error after update | Open | Critical | Member A |
| ISS-002 | Report page freezes | In Progress | High | Member B |
| ISS-003 | Incorrect date formatting | Resolved | High | Member A |
| ISS-004 | Export unavailable | Closed | Critical | Member B |
| ISS-005 | Message spacing issue | Open | Normal | Unassigned |
| ISS-006 | Tooltip wording unclear | Open | Not Set | Unassigned |
| ISS-007 | File upload fails | Resolved | Critical | Lead |
| ISS-008 | Button label truncated | In Progress | Low | Member A |

Important content: ISS-001, ISS-002, ISS-003, ISS-007.

## M-01 — Create Issue overlay [US-01; D-01, D-04]

Inputs: Title C-05, required; Description C-06, required multiline. Actions: Create C-07 primary and Cancel C-08 secondary. Only these two issue fields are editable here.

After success: generated ID, Open status, absent priority shown as Not Set, Unassigned assignee, current authenticated author, generated creation time. Do not expose these defaults as inputs in M-01.

Show default, missing-Title and missing-Description examples. Invalid submission does not create an issue. Preserve the source example “Title is required”; working validation copy for Description is a design choice, not a new source requirement.

Do not include inputs for status, priority, author, ID, assignee, attachments.

## S-03 — Issue Details [US-02–US-07]

Show ID, Title, Description, Status, Priority, Assignee, Author, Created at. Unassigned/Not Set are empty-value display states, not extra metadata. Provide Return to Issues List C-17.

Keep Assignee, Priority and Status controls together without a decorative “Control Actions Area” heading. Canonical example: Lead viewing ISS-003 assigned to Member A; assignment C-09/C-10 and priority C-11/C-12 enabled, status C-13/C-14 disabled.

Only Lead can assign/reassign one team member and set/change priority [US-03; AC-04.2; D-04]. Member controls are disabled [D-05]. Selection is applied with an Apply action adjacent to that control [D-06]; the visible result remains on S-03.

Priority display values: Not Set, Low, Normal, High, Critical. Selectable levels: Low, Normal, High, Critical only. Not Set represents absence, not a selectable level or a reset action [BR-04/05; D-03].

Only the current assignee can change status, including when the assignee is Lead [AC-07.4]. Allowed transitions only [BR-08.1]:
- Open → In Progress
- In Progress → Resolved
- Resolved → In Progress
- Resolved → Closed

In Closed there is no next status action. Show current status but do not offer it as a new transition. Resolved is resolved but not closed [BR-09.1]. Do not infer that Closed disables unrelated features (comments and priority rules do not change in Closed).

Comments: C-15 Comment input (textarea), C-16 Add Comment action. Saved entries show text, author and creation date/time. Empty submission shows validation and creates nothing [AC-06.1]. Save failure shows an error and no saved comment [AC-06.5]. Include ready, validation, saved and failure examples.

## REF-01 — state reference outside product frames

Show all four role contexts: Member non-assignee; Member assignee; Lead non-assignee; Lead assignee. Represent assignment/priority/status control availability for each. Critical example to include: Lead non-assignee — assignment enabled, priority enabled, status disabled.

Also show all four status examples (Open, In Progress, Resolved, Closed) with valid destinations only. No assignee means no user can change status before assignment.

Use compact components or annotated examples; do not create 16 complete detail screens. Visible annotations specify the test actor, issue ID and current status; they are not product controls or a role-switcher feature.

## Navigation intent [D-01/02/07/08]

S-02 tabs switch list states; issue Title opens the corresponding S-03; Create Issue opens M-01; Cancel returns to the calling state; successful Create shows the prepared new issue on S-03; Return restores the prior list state (either All or Important depending on origin). Assignment, priority, status and comments remain on S-03 after success. Invalid operations do not show a success state. These are design intentions, not working persistence.

## Exclusions

No title/description editing after creation, deletion, attachments, AI features, dashboards, analytics, notifications, search, sorting, pagination, user administration, user-visible history, extra issue metadata, multi-assignee, extra status/priority values or transitions, invented login method, row selection, bulk actions, branding or decorative imagery. No separate product screens for assignment, priority, status, comments or Important Issues.

## Acceptance checklist

The design is complete when:
- 4 canonical product screens + REF-01 are present (S-01, S-02-A, S-02-B, S-02-C, M-01, S-03, REF-01).
- All 33 AC trace to a concrete screen/state/control (see Traceability).
- Create inputs are exactly Title + Description; validation examples exist.
- New issue defaults: Open, Not Set, Unassigned, current author, creation time.
- All 4 allowed status transitions are shown; no other transitions exist; Closed has no outgoing.
- Important list contains ISS-001, ISS-002, ISS-003, ISS-007 and excludes ISS-004 (Closed) and ISS-006 (Not Set).
- Lead non-assignee example shows status disabled.
- Member assignee example shows status enabled (for allowed transitions).
- Comment states shown: ready, validation, saved, save-failure.
- No decorative page icons, placeholders, row-selection, bulk controls, Delete, Search, Sort, Analytics.
- Reviewer annotations are outside product frames.
- Layers are native and editable; reusable local components for Button, Field, Badge, Row, Dropdown.

## Completion report

In the same Agent response report: created frames, actual reusable components, requirements not represented, assumptions and limitations. State uncertainty instead of claiming full compliance. A static wireframe does not prove authentication, server permissions, saved data, response time, HTTPS or backup.
