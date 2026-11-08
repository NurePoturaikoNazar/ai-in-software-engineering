# 03 Screen Contracts v1.0 — G3

Проєкт: Mini Issue Tracker. Дата: 08.11.2026. Статус: PROPOSED → чекає перевірки G4.

Developer-Verifiable Screen Contracts. Нові UX-рішення — PROPOSED D-ID, не SOURCE.

## Permission Matrix

|Дія|Member non-assignee|Member assignee|Lead non-assignee|Lead assignee|Source|
|---|---|---|---|---|---|
|View issue (list & details)|✓|✓|✓|✓|AC-02.1..AC-02.4|
|Create issue|✓ *|✓ *|✓|✓|US-01 (* — підтвердити Q-01)|
|Add comment|✓|✓|✓|✓|US-06|
|Assign / reassign|✗|✗|✓|✓|US-03 п.3; BR-06|
|Change priority|✗|✗|✓|✓|AC-04.2; BR-06|
|Change status|✗|✓ (за BR-08.1)|✗|✓ (за BR-08.1)|AC-07.4; BR-08; BR-08.1|

> **Критичний випадок:** Lead non-assignee **НЕ** може змінити статус. AC-07.4 вище за будь-які «керівні» припущення.

## Status Transition Matrix

|З \ До|Open|In Progress|Resolved|Closed|
|---|---|---|---|---|
|Open|поточний|✓|✗|✗|
|In Progress|✗|поточний|✓|✗|
|Resolved|✗|✓|поточний|✓|
|Closed|✗|✗|✗|поточний (немає вихідних переходів)|

**4 дозволених переходи (BR-08.1):**
1. Open → In Progress
2. In Progress → Resolved
3. Resolved → In Progress
4. Resolved → Closed

> Перехід виконує лише `current user = current assignee`. Якщо `assignee = null`, **ніхто** не може змінити status до призначення.

## Important predicate

```
Important(issue) = (issue.priority ∈ {High, Critical}) AND (issue.status ≠ Closed)
```

Приклади:
- High + Resolved → **Important** (ISS-003)
- Critical + Closed → **НЕ** Important (ISS-004)
- Not Set + Open → **НЕ** Important (ISS-006). Відсутність priority ≠ High/Critical.
- Normal + Open → **НЕ** Important.

## Screen Contract: S-01 — Authentication boundary

|Поле|Значення|
|---|---|
|ID|S-01|
|Purpose|Нейтральна межа входу. Без реальної авторизації.|
|Actors|будь-який користувач|
|Entry|перший візит|
|Visible data|назва застосунку, рядок «Authentication entry — mechanism TBD»|
|Controls|C-00 Continue → перехід на S-02-A як імітований user (для demo)|
|States|default|
|Errors|—|
|Outgoing|T-01 → S-02-A|
|Source|BR-01; D-02|

## Screen Contract: S-02 — Issues List

|Поле|Значення|
|---|---|
|ID / States|S-02-A (All), S-02-B (Important), S-02-C (Empty All)|
|Purpose|Перегляд issues і навігація до деталей / створення нової.|
|Actors|Member, Lead|
|Entry|T-01, T-05, T-19|
|Visible data (рядок)|ID, Title, Status, Priority (або «Not Set»), Assignee (або «Unassigned»)|
|Visible data (header)|назва «Issues List», лічильник (опційно), tabs|
|Controls|C-01 Tab All; C-02 Tab Important; C-03 Create Issue (button); C-04 Row (клікабельний Title)|
|Required|немає user input'ів|
|Role conditions|C-01, C-02, C-04 — усі ролі; C-03 — див. Q-01|
|Default (A)|усі issues|
|Default (B)|фільтр Important predicate|
|Empty (C)|«No issues yet», tabs і Create Issue залишаються видимими|
|Validation|—|
|Errors|—|
|Outgoing|T-02, T-03 (між станами), T-04 (Create), T-09 (деталі)|
|Source|AC-02.1..AC-02.5; AC-05.3; AC-08.1..AC-08.4; D-01, D-07|

## Screen Contract: M-01 — Create Issue overlay

|Поле|Значення|
|---|---|
|ID|M-01|
|Purpose|Створити нову issue.|
|Actors|Member, Lead (підтвердити Q-01)|
|Entry|T-04|
|Visible data|labels «Title», «Description»|
|Controls|C-05 Title (text input, required); C-06 Description (textarea, required); C-07 Create (primary); C-08 Cancel (secondary)|
|Default|порожні поля|
|Validation|Title ≠ ∅; Description ≠ ∅|
|Errors|«Title is required» під C-05; «Description is required» під C-06|
|Success|нова issue: ID (generated), status = Open, priority = Not Set, assignee = Unassigned, author = current user, created_at = now|
|Outgoing|T-05 (Cancel), T-06 (valid), T-07/T-08 (validation)|
|Source|AC-01.1..AC-01.5; D-01, D-04|

> **Не додавати** в M-01: priority, status, assignee, author, ID, attachments як input'и. Priority/Assignee встановлюються лише з S-03.

## Screen Contract: S-03 — Issue Details

|Поле|Значення|
|---|---|
|ID|S-03|
|Purpose|Повна інформація, керування assignee/priority/status, коментарі.|
|Actors|Member, Lead|
|Entry|T-06 (нова), T-09 (з рядка списку)|
|Visible data|ID, Title, Description, Status, Priority (або «Not Set»), Assignee (або «Unassigned»), Author, Created at|
|Controls|C-09 Assignee dropdown + C-10 Apply Assignee; C-11 Priority dropdown + C-12 Apply Priority; C-13 Status dropdown + C-14 Apply Status; C-15 Comment textarea + C-16 Add Comment; C-17 Return to Issues List|
|Visibility (C-09, C-10)|enabled лише для Lead; disabled для Member|
|Visibility (C-11, C-12)|enabled лише для Lead; disabled для Member|
|Visibility (C-13, C-14)|enabled лише коли `current user = current assignee` AND існує ≥1 дозволений перехід; disabled для non-assignee; disabled для Closed (немає вихідних переходів); disabled для будь-кого якщо assignee = null|
|C-13 options|лише дозволені наступні status для поточного (див. Transition Matrix)|
|C-11 options|Low, Normal, High, Critical. «Not Set» — не selectable option, лише індикація|
|Validation (comment)|text ≠ ∅|
|States (comments)|empty (initial), validation-error, saved-success, save-failure|
|Errors|«Comment text is required» (empty); «Failed to save comment» (save failure)|
|Saved comment entry|text, author, timestamp|
|Outgoing|T-10..T-18, T-19|
|Source|AC-02.4; AC-04.1, AC-04.2; AC-05.1, AC-05.2; AC-06.1..AC-06.5; AC-07.1..AC-07.5; BR-04/05/06/08/08.1/09.1; D-03..D-06|

## REF-01 — State reference (поза продуктовими frames)

Містить компактні приклади для розробника/рецензента:
- Повна Permission Matrix (вище).
- Повна Status Transition Matrix (вище).
- Приклади для 4 статусів у різних ролях (Member/Lead × assignee/non-assignee).
- Comment states (ready, validation, saved, failure).
- Priority display states (Not Set, Low, Normal, High, Critical).

**Не є частиною продуктового UI** — розміщується у секції «Review notes» поза продуктовими frames.

## Traceability Matrix

|Source|Доказ у дизайні / Screen|Control / annotation|Test ID|Verification level|
|---|---|---|---|---|
|AC-01.1|M-01|C-05, C-06|T-01|wireframe|
|AC-01.2|M-01 (validation state)|C-05 + error|T-02|wireframe|
|AC-01.3|M-01 (validation state)|C-06 + error|T-02|wireframe|
|AC-01.4|S-03 (new issue)|header + метадані|T-01|wireframe+prototype|
|AC-01.5|S-03 (new issue)|Priority = «Not Set»|T-01|wireframe|
|AC-02.1|S-02-A|C-04 rows|T-08|wireframe|
|AC-02.2|S-02-A|row поля|T-08|wireframe|
|AC-02.3|T-09|row click → S-03|T-08|prototype|
|AC-02.4|S-03|усі поля|T-08|wireframe|
|AC-02.5|S-02-C|empty-state text|T-08|wireframe|
|US-03 п.1|S-03|C-09 single-select|T-03|wireframe|
|US-03 п.2|S-03|C-09 options = members|T-03|wireframe|
|US-03 п.3|S-03|C-09/10 enabled лише Lead|T-03|wireframe|
|US-03 п.4|S-03|Assignee display|T-03|wireframe|
|AC-04.1|S-03|C-11 options|T-04|wireframe|
|AC-04.2|S-03|C-11/12 enabled лише Lead|T-04|wireframe|
|AC-05.1|S-02, S-03|Status columns/fields|T-08|wireframe|
|AC-05.2|S-03|Status display|T-08|wireframe|
|AC-05.3|S-02|Status у row|T-08|wireframe|
|AC-06.1|S-03|validation state|T-07|wireframe|
|AC-06.2|S-03|saved entry list|T-07|prototype|
|AC-06.3|S-03|author у saved entry|T-07|wireframe|
|AC-06.4|S-03|time у saved entry|T-07|wireframe|
|AC-06.5|S-03|failure state|T-07|wireframe|
|AC-07.1|S-03 (new)|Status = Open|T-01|wireframe|
|AC-07.2|REF-01 / C-13 options|перелік 4 статусів|T-05|wireframe|
|AC-07.3|S-03 (after Apply)|оновлений Status|T-05|prototype|
|AC-07.4|S-03 (F-06/F-07)|C-13 disabled для non-assignee|T-05/T-06|wireframe|
|AC-07.5|C-13 options|лише дозволені переходи|T-05|wireframe|
|AC-08.1|S-02-B|ISS-001/002/003/007 видимі|T-08|wireframe|
|AC-08.2|S-02-B|Critical включено|T-08|wireframe|
|AC-08.3|S-02-B|Low/Normal виключено|T-08|wireframe|
|AC-08.4|S-02-B|Closed виключено (ISS-004)|T-08|wireframe|
|BR-01|S-01|placeholder-boundary|NOT VERIFIABLE у wireframe|реалізація|
|BR-08.1|REF-01 transition examples|4 переходи|T-05|wireframe|
|BR-09.1|S-02-B з ISS-003|High+Resolved у Important|T-08|wireframe|
|NFR-02/03/04/05|—|—|NOT VERIFIABLE|реалізація/deployment|

## Tests (анкета для розробника)

|Test ID|Actor / Context|Precondition|Action|Expected visible result|Що НЕ доводиться|
|---|---|---|---|---|---|
|T-01 Create|Member/Lead; S-02|—|Create Issue → M-01 → ввести Title+Description → Create|S-03 нової issue: Open, Not Set, Unassigned, author=current, time=now|persistence у БД|
|T-02 Validation|Member/Lead; M-01|—|Create з порожнім Title / Description|error «… is required»; issue не створено|серверна валідація|
|T-03 Assignment|Member, Lead; S-03|issue існує|Member: C-09 disabled. Lead: обрати member → Apply|Member: нічого. Lead: Assignee оновлено|авторизація на сервері|
|T-04 Priority|Member, Lead; S-03|issue існує|Member: C-11 disabled. Lead: обрати level → Apply|Member: нічого. Lead: Priority оновлено|серверна валідація|
|T-05 Status (allowed)|current assignee; S-03|current status має дозволений перехід|обрати valid next → Apply|Status оновлено; у S-02 оновлено|persistence|
|T-06 Lead restriction|Lead non-assignee; S-03|issue з assignee ≠ Lead|перевірити C-13|C-13 disabled|авторизація|
|T-07 Comments|Member/Lead; S-03|—|empty → Add → validation. valid → Add → saved. simulated failure → Add → error|три окремі стани|transactional save|
|T-08 Important|Member/Lead; S-02|baseline data|відкрити Important tab|у списку ISS-001, 002, 003, 007|реальний запит до БД|
|T-09 Navigation|Member/Lead; S-03|увійшли з Important|Return|повернутись на Important (D-07)|—|
|T-10 Scope|Member/Lead|—|оглянути макет цілком|немає Delete, Attachments, AI, Search, Sort, History, bulk, page icon placeholder|—|

## Business decisions required

Перед F1 мають бути закриті:
- **Q-01** (хто створює issue). Варіанти: (a) обидва; (b) лише Lead; (c) лише Member. Поточне припущення — (a).
- **Q-05** (bootstrapping без assignee). Варіанти: (a) Lead перепризначає; (b) ніхто не чіпає status до assign. Поточне припущення — (a).

Решта — питання реалізації, не макета.
