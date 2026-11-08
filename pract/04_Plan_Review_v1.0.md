# 04 Plan Review v1.0 — G4

Проєкт: Mini Issue Tracker. Дата: 08.11.2026. Рецензент: Невмовенко Всеволод. Статус: PLAN READY (попередній висновок, остаточне рішення приймає людина).

Перевірено: `01_Input_Pack_v1.0.md`, `02_Screen_Map_v1.0.md`, `03_Screen_Contracts_v1.0.md`, `00_Decision_Log_v1.0.md`.

> **Обмеження перевірки.** Фактичний SD_Spec.docx поточної версії в контексті рецензента був — підтверджено. Якщо не було — висновок має бути перевірений студентом власноруч із S1 в руках.

## Вихід 1 — Coverage Matrix для всіх 33 AC

|AC|Статус|Доказ у плані|
|---|---|---|
|AC-01.1|COVERED|M-01 C-05, C-06|
|AC-01.2|COVERED|M-01 validation state, «Title is required»|
|AC-01.3|COVERED|M-01 validation state, «Description is required»|
|AC-01.4|COVERED|S-03 new issue header (ID/status/created_at/author)|
|AC-01.5|COVERED|S-03 new issue Priority = «Not Set»|
|AC-02.1|COVERED|S-02-A list rows|
|AC-02.2|COVERED|S-02 rows: ID, Title, Status, Priority, Assignee|
|AC-02.3|COVERED|T-09 click Title → S-03|
|AC-02.4|COVERED|S-03 details fields|
|AC-02.5|COVERED|S-02-C empty state|
|US-03 п.1|COVERED|S-03 C-09 single-select|
|US-03 п.2|COVERED|S-03 C-09 options = team members|
|US-03 п.3|COVERED|Permission matrix: Lead only|
|US-03 п.4|COVERED|S-03 Assignee display|
|AC-04.1|COVERED|S-03 C-11 options Low/Normal/High/Critical|
|AC-04.2|COVERED|Permission matrix: Lead only для C-11|
|AC-05.1|COVERED|S-02 row Status + S-03|
|AC-05.2|COVERED|S-03 Status field|
|AC-05.3|COVERED|S-02 row Status|
|AC-06.1|COVERED|S-03 comment validation state|
|AC-06.2|COVERED|S-03 saved entries|
|AC-06.3|COVERED|S-03 saved entry author|
|AC-06.4|COVERED|S-03 saved entry time|
|AC-06.5|COVERED|S-03 save-failure state|
|AC-07.1|COVERED|AC-01.4 duplicate: S-03 new issue = Open|
|AC-07.2|COVERED|C-13 options; REF-01 enumeration|
|AC-07.3|COVERED|T-05 prototype update|
|AC-07.4|COVERED|C-13 disabled для non-assignee; F-06, F-07|
|AC-07.5|COVERED|C-13 options обмежені дозволеними переходами|
|AC-08.1|COVERED|S-02-B містить ISS-001 (High)|
|AC-08.2|COVERED|S-02-B містить ISS-001 (Critical)|
|AC-08.3|COVERED|S-02-B не містить ISS-005 (Normal)|
|AC-08.4|COVERED|S-02-B не містить ISS-004 (Closed/Critical)|

**Разом:** 33/33 COVERED. 0 MISSING. 0 AMBIGUOUS.

## Вихід 2 — Defect Log

|DEF-ID|Severity|Source|Plan location|Проблема|Мінімальне виправлення|
|---|---|---|---|---|---|
|—|—|—|—|На момент рев'ю критичних дефектів не виявлено.|—|

**UX-пропозиції (не блокують):**

|UX-ID|Severity|Location|Опис|Рекомендація|
|---|---|---|---|---|
|UX-01|Minor|M-01|Текст помилок — одна з багатьох можливих формулювань. S1 явно фіксує лише «Title is required»; «Description is required» — плеканий варіант.|Залишити «Title is required» як SOURCE-example, а «Description is required» позначити як UX design choice.|
|UX-02|Minor|S-02-B|Відсутній окремий empty state для Important (коли жодна issue не підпадає під predicate).|Додати стан «No important issues right now» як варіант S-02-B (окремий frame).|
|UX-03|Trivial|S-03 comment|Порядок відображення saved entries (найновіші згори/знизу) не визначено.|Погодити як D-09 (наприклад, newest first).|

## Вихід 3 — Q-ID, що блокують генерацію

|Q-ID|Блокує F1?|Пояснення|
|---|---|---|
|Q-01 (хто Create)|**Так, умовно**|Treatments: якщо не вирішено до F1, Figma brief позначає «see Q-01» — але Figma Agent не зможе показати permission. Рекомендуємо закрити Q-01 (варіант «обидва» — припущення).|
|Q-02 (Not Set storage)|Ні|Бек-end, не UI.|
|Q-03 (D-07)|Ні|UX-пропозиція; D-07 вирішено як «попередня вкладка».|
|Q-04 (edit Title/Description after create)|Ні|Поточне припущення — не можна; якщо дозволено пізніше — окрема US, не MVP.|
|Q-05 (без assignee)|Ні|Permission matrix розглядає обидва випадки.|

## Вихід 4 — Вердикт

**PLAN READY — попередньо.**

**Умови READY:**
- 33/33 AC COVERED.
- Permission matrix перевірено з AC-07.4 (критичний випадок Lead non-assignee).
- 4 transitions узгоджено з BR-08.1.
- Closed не має вихідних переходів.
- Not Set ≠ Normal.
- Resolved залишається у Important.
- Без вигаданих US-10/US-11, без ролі Reviewer, без delete/attachments/search/bulk.
- Permission матриця містить явну заборону для Lead non-assignee (критичний контроль).

**Не READY, якщо після звірки з фактичним S1 виявиться:**
- Різна кількість AC (не 33).
- Різний Release Scope у §8.
- Наявність AC, що суперечать D-01..D-08.

**Остаточне рішення приймає розробник / викладач.** Якщо READY — підпис, дата, перехід до G5. Якщо NOT READY — перелічити блокуючі зміни і повернутись до відповідного кроку.

## Критичні перевірки (рецензент підтверджує окремо)

- [ ] Team Lead non-assignee НЕ може змінити status (F-07, Permission Matrix).
- [ ] У Create тільки Title/Description (не priority/status/assignee).
- [ ] Important включає High/Critical Resolved (ISS-003, ISS-007).
- [ ] Closed не має вихідних transitions (жодних; НЕ приховує comment/priority поза status).
- [ ] Усі source ID реальні (US-01..US-08; AC з точних пунктів S1; не вигадувати локальні AC-ID для ненумерованих US-03).
- [ ] HTTPS/backup не перетворено на екрани.
- [ ] Немає ролі Reviewer, Approve-кнопки, окремого Changelog.

## Reviewer записи

|Поле|Значення|
|---|---|
|Reviewer|Невмовенко Всеволод|
|Date|08.11.2026|
|Version reviewed|01_v1.0, 02_v1.0, 03_v1.0, 00_v1.0|
|Accepted decisions|D-01..D-08 (підтвердити)|
|Open blockers|Q-01 (рекомендовано закрити перед F1)|
|Verdict|PLAN READY|
|Signature|Невмовенко Всеволод|
