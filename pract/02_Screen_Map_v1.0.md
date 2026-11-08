# 02 Screen Map v1.0 — G2

Проєкт: Mini Issue Tracker. Дата: 08.11.2026. Статус: PROPOSED → чекає перегляду в G4.

Базується на: `01_Input_Pack_v1.0.md`, `00_Decision_Log_v1.0.md` (D-01..D-08).

## Screen Inventory (продуктові поверхні)

|Screen ID|Name|Purpose|Actors|Entry|
|---|---|---|---|---|
|S-01|Authentication boundary|Нейтральна межа «вхід до застосунку»; механізм не визначено.|усі|перший візит|
|S-02|Issues List|Перегляд усіх і важливих заявок.|Member, Lead|з S-01 після імітованої автентифікації; з M-01 Cancel; з S-03 Return|
|S-02-A|State — All Issues|Усі зареєстровані issues.|Member, Lead|клік по вкладці All|
|S-02-B|State — Important Issues|Високий пріоритет, незакриті.|Member, Lead|клік по вкладці Important|
|S-02-C|State — Empty All|Нуль зареєстрованих заявок.|Member, Lead|перший старт застосунку / порожня БД|
|M-01|Create Issue overlay|Створити нову заявку.|Member, Lead (підтвердити Q-01)|клік по Create Issue на S-02|
|S-03|Issue Details|Повна інформація, assignment/priority/status, коментарі.|Member, Lead|клік по Title рядка у S-02|
|REF-01|State reference (поза продуктовими frames)|Службові приклади: permission matrix, transition matrix, state examples.|розробник / рецензент|не досяжно з прототипу користувача|

**Не створювати** як окремі продуктові сторінки: Assignment, Priority, Status, Comments, Important (це стан списку, а не окрема сторінка), Change Priority, Delete, Attachments, Search, Sort, Dashboard, Notifications, User Admin, History.

## Figma frames count vs product screens

Кількість продуктових поверхонь: **4** (S-01, S-02, M-01, S-03) + 1 службова секція REF-01.
Очікувана кількість Figma frames: **6+** через окремі стани одного екрана (S-02-A / S-02-B / S-02-C), приклади помилок у M-01, success/validation/save-error для коментарів у S-03. Це не порушує правило «Important — не окремий екран».

## Visible data / Inputs / Actions per screen

### S-01 — Authentication boundary

- **Visible data:** назва застосунку; напис «Authentication entry — mechanism TBD».
- **Inputs:** немає.
- **Actions:** _симуляція_: Continue → S-02-A з даними імітованого користувача.
- **Role/state conditions:** —
- **Errors/empty states:** —
- **Source:** S1 BR-01; D-02.

### S-02 — Issues List (загальні поля)

- **Header:** назва «Issues List»; action Create Issue.
- **Tabs:** All Issues / Important Issues.
- **Row поля:** ID, Title, Status, Priority, Assignee (або порожнє — «Unassigned»).
- **Inputs:** вибір tab; клік по Title.
- **Actions:** Create Issue → відкриває M-01; клік по рядку → S-03.
- **Role/state conditions:** усі ролі бачать і All, і Important однаково.
- **Errors/empty states:** S-02-C (empty All); окремо показати empty Important (можна як стан того ж екрана).
- **Source:** AC-02.1..AC-02.3, AC-02.5; AC-05.3; AC-08.1..AC-08.4; D-01, D-07.

**S-02-A (All):** усі зареєстровані issues, за будь-яким priority.
**S-02-B (Important):** фільтр `(priority ∈ {High, Critical}) AND (status ≠ Closed)`. Resolved включається.
**S-02-C (Empty All):** нуль зареєстрованих issues; текст «No issues yet» + видимі tabs і Create Issue.

### M-01 — Create Issue overlay

- **Visible data:** лейбли полів.
- **Inputs:** Title (обов'язкове), Description (обов'язкове, multiline).
- **Actions:** Create (валідна форма → S-03 з новою issue), Cancel (закриває overlay, повертає на попередній стан S-02).
- **Role/state conditions:** доступний після Q-01; поточне припущення — обом ролям.
- **Errors/empty states:** validation error якщо Title порожній («Title is required»); validation error якщо Description порожній; valid-success stub із новим ID.
- **Source:** AC-01.1..AC-01.5; D-01, D-04.

> **Не додавати** у M-01 input'и для: status, priority, author, ID, assignee, attachments.

### S-03 — Issue Details

- **Visible data:** ID, Title, Description, Status, Priority, Assignee, Author, Created at.
- **Inputs:**
  - Assignee: dropdown members → Apply (видимість: Lead enabled; Member disabled).
  - Priority: dropdown {Low, Normal, High, Critical} → Apply (видимість: Lead enabled; Member disabled). Not Set — лише як індикація поточного стану.
  - Status: dropdown дозволених переходів для поточного current assignee → Apply (видимість: current assignee enabled; інші disabled; для Closed — немає дозволених наступних станів, тож control disabled/no-options).
  - Comment input: textarea + Add Comment action.
- **Actions:** Apply Assignee / Apply Priority / Apply Status (кожна — окрема кнопка поруч із control); Add Comment; Return to Issues List.
- **Role/state conditions:** detailed матриця — див. `03_Screen_Contracts_v1.0.md`.
- **Errors/empty states:**
  - Empty comment → validation error під полем; saved entry не створюється.
  - Comment save failure → error-banner; saved entry не створюється.
  - Assignee = Unassigned → display label.
  - Priority = Not Set → display label.
- **Source:** AC-02.4; AC-04.1, AC-04.2; AC-05.1, AC-05.2; AC-06.1..AC-06.5; AC-07.1..AC-07.5; BR-04/05/06/08/08.1/09.1; D-03, D-04, D-05, D-06.

> **Не створювати** окремі сторінки для Assign, Priority, Status, Comments — усе на S-03.

## Transition table

|T-ID|From|Actor|Trigger|Preconditions|To|Visible result|Requirement / D-ID|
|---|---|---|---|---|---|---|---|
|T-01|S-01|Member/Lead|Continue|імітація автентифікації|S-02-A|повний список|BR-01; D-02|
|T-02|S-02-A|Member/Lead|клік по вкладці Important|немає|S-02-B|відфільтрований список|AC-08.1..AC-08.4; D-01|
|T-03|S-02-B|Member/Lead|клік по вкладці All|немає|S-02-A|повний список|AC-02.1; D-01|
|T-04|S-02-A/B|Member/Lead|клік по Create Issue|немає|M-01|відкрито overlay|US-01; D-01|
|T-05|M-01|Member/Lead|Cancel|немає|попередній S-02-A або B|overlay закрито, issue не створено|D-07|
|T-06|M-01|Member/Lead|Create з валідними полями|Title ≠ ∅, Description ≠ ∅|S-03 (new issue)|нова issue Open, no priority, no assignee|AC-01.1..AC-01.5|
|T-07|M-01|Member/Lead|Create без Title|Title = ∅|M-01 з error|«Title is required»; issue не створено|AC-01.2|
|T-08|M-01|Member/Lead|Create без Description|Description = ∅|M-01 з error|«Description is required»; issue не створено|AC-01.3|
|T-09|S-02|Member/Lead|клік по Title рядка|rowId дійсний|S-03|повні деталі issue|AC-02.3|
|T-10|S-03|Lead|Apply Assignee|вибір зроблено|S-03 (оновлений)|Assignee оновлено|US-03 п.3; D-04/06|
|T-11|S-03|Lead|Apply Priority|вибір зроблено|S-03 (оновлений)|Priority оновлено|AC-04.2; D-06|
|T-12|S-03|current assignee|Apply Status (Open → In Progress)|current status = Open|S-03 (In Progress)|Status оновлено|BR-08.1; AC-07.4|
|T-13|S-03|current assignee|Apply Status (In Progress → Resolved)|current status = In Progress|S-03 (Resolved)|Status оновлено|BR-08.1|
|T-14|S-03|current assignee|Apply Status (Resolved → In Progress)|current status = Resolved|S-03 (In Progress)|Status оновлено|BR-08.1|
|T-15|S-03|current assignee|Apply Status (Resolved → Closed)|current status = Resolved|S-03 (Closed)|Status оновлено|BR-08.1|
|T-16|S-03|Member/Lead|Add Comment (valid)|text ≠ ∅|S-03 (саме поле + saved entry)|comment додано|AC-06.2..AC-06.4|
|T-17|S-03|Member/Lead|Add Comment (empty)|text = ∅|S-03 з error|validation; saved entry не додано|AC-06.1|
|T-18|S-03|Member/Lead|Add Comment (server error)|save failed|S-03 з error-banner|помилка; saved entry не додано|AC-06.5|
|T-19|S-03|Member/Lead|Return to Issues List|немає|попередній стан S-02-A/B|контекст збережено|D-07|

**Недопустимі дії не створюють переходів:**
- Member non-assignee клікає Apply Status — control disabled, перехід не стартує.
- Member клікає Apply Priority — control disabled.
- Будь-хто для Closed issue клікає Apply Status — доступних переходів немає.
- Transition Open → Resolved (пропускаючи In Progress) — не існує.
- Transition Closed → будь-що — не існує.

## User flows (текстом)

**F-01 Основний сценарій створення:**
S-01 → S-02-A → клік Create Issue → M-01 → ввести Title+Description → Create → S-03 нової issue (Open, Not Set, Unassigned) → Return → S-02-A (нова issue у списку).

**F-02 Перегляд важливих:**
S-02-A → клік вкладки Important → S-02-B (ISS-001, 002, 003, 007 за baseline даними) → клік по Title → S-03 → Return → S-02-B (повернулись на Important, не на All).

**F-03 Призначення Lead'ом:**
S-02-A → клік на рядку з ISS-005 (Unassigned) → S-03 → Lead обирає Member A у dropdown Assignee → Apply → S-03 оновлений.

**F-04 Зміна priority Lead'ом:**
S-03 (будь-яка issue) → Lead обирає High у Priority → Apply → S-03 оновлений. (Якщо issue була в All з Normal і тепер High — у S-02-B вона з'явиться.)

**F-05 Валідна зміна статусу:**
Member A як assignee ISS-002 (In Progress) → S-03 → Status dropdown → Resolved → Apply → S-03 (Resolved); потім Status → Closed → Apply → S-03 (Closed). У S-02-B ISS-002 зникає.

**F-06 Заборонена зміна статусу (Member non-assignee):**
Member B відкриває ISS-002 (assignee = Member A) → S-03 → Status dropdown **disabled** (видно поточний status, але змінити не можна). Ніякого переходу.

**F-07 Заборонена зміна статусу (Lead non-assignee):**
Lead відкриває ISS-002 (assignee = Member A) → S-03 → Assignee enabled (Lead може перепризначити), Priority enabled, **Status disabled** (бо Lead не є assignee цієї issue). Критичний приклад.

**F-08 Comment success:**
S-03 → ввести «Added details about reproduction» → Add Comment → saved entry з author/time відображається під input.

**F-09 Comment validation:**
S-03 → порожнє поле → Add Comment → error під полем; saved entry не додано.

**F-10 Comment server error:**
S-03 → ввести коментар → Add Comment → error-banner «Failed to save comment»; saved entry не додано.

**F-11 Empty state:**
Застосунок з нульовими даними → S-02-C «No issues yet» → Create Issue → M-01 → valid → S-03 нової issue → Return → S-02-A з однією issue.

## 3 приклади того, що НЕ слід додавати

1. **Search / Filter / Sort над списком** — немає в S1, не входить у MVP, типово для issue trackers але тут заборонено.
2. **Row-selection checkbox / Bulk Delete** — BR-10 забороняє delete взагалі; bulk-дії поза scope.
3. **Reviewer role або окрема дія «Approve» для переходу Resolved → Closed** — S1 не вводить таку роль; перехід виконує current assignee.

## Unresolved в плані

- Q-01 — хто створює issue (див. Input Pack). План M-01 припускає обох, поки не підтверджено.
- Q-05 — хто може працювати з issue без assignee (bootstrapping). План припускає, що Lead перепризначає.
- D-07 — повернення на попередню вкладку: припущено як поведінка; якщо не підтверджено, план повертає на S-02-A.

## Що план ще не покриває (навмисно)

- Візуальну оформлення (кольори, типографіка) — low-fi grayscale.
- Пагінацію / lazy load — поза scope.
- Пошук / фільтр — поза scope.
- Експорт — поза scope.
- Реальний API / збереження — поза макетом.
