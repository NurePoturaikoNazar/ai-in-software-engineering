# 06 Review Report — G6 (ШАБЛОН)

Проєкт: Mini Issue Tracker. Дата перевірки: 08.11.2026. Оператор: _[ПІБ]_. Статус: ЧЕРНЕТКА — заповнюється після F1/F2 та огляду скриншотів.

> **Коли заповнювати цей файл.** Після того як завершено F1 (та F2 якщо був), створено прототип (P1), зроблено screenshots A (list + modal) і B (details + state reference) з читабельними Frame/State IDs, і ці скриншоти + коротке повідомлення передано ChatGPT з промптом з розділу 19 методички.

## 0 — Контекст

|Поле|Значення|
|---|---|
|Figma file link|_[https://www.figma.com/design/...]_|
|Робоча версія|_[назва, напр. v1.2]_|
|Screenshots передані AI|A: _[файл]_; B: _[файл]_|
|Нечіткі / відсутні докази|_[перелічити або «немає»]_|
|AI-інструмент|_[ChatGPT model]_ / Vision|
|Дата генерації звіту|_[дд.мм.рррр]_|

## Частина A — Вимоги

> **Правило.** PASS — очікуваний UI-доказ видно; PARTIAL — видно лише частину; FAIL — видно суперечність; NOT VERIFIABLE — даних недостатньо або вимога стосується реалізації.

|Source|Expected|Screen/State|Observed evidence|Result|Remaining test|
|---|---|---|---|---|---|
|AC-01.1|Title + Description inputs у Create|M-01 (screenshot A)|_[опис що видно]_|_[PASS/…]_|—|
|AC-01.2|«Title is required» error|M-01 validation state|_[…]_|_[…]_|—|
|AC-01.3|«Description is required» error|M-01 validation state|_[…]_|_[…]_|—|
|AC-01.4|New issue: Open/author/created_at/ID|S-03 (new)|_[…]_|_[…]_|persistence — на реалізацію|
|AC-01.5|Priority = «Not Set» після створення|S-03 (new)|_[…]_|_[…]_|—|
|AC-02.1|Список issues|S-02-A (screenshot A)|_[…]_|_[…]_|—|
|AC-02.2|Row fields: ID/Title/Status/Priority/Assignee|S-02-A rows|_[…]_|_[…]_|—|
|AC-02.3|Клік по Title → S-03|потрібна перевірка прототипу|_[…]_|_[…]_|P1 click test|
|AC-02.4|Details fields|S-03 (screenshot B)|_[…]_|_[…]_|—|
|AC-02.5|Empty state «No issues yet»|S-02-C|_[…]_|_[…]_|—|
|US-03 п.1|single-select assignee|S-03 C-09|_[…]_|_[…]_|—|
|US-03 п.2|options = team members|S-03 C-09|_[…]_|_[…]_|—|
|US-03 п.3|лише Lead enabled|REF-01 + S-03|_[…]_|_[…]_|серверна авторизація|
|US-03 п.4|Assignee display|S-03|_[…]_|_[…]_|—|
|AC-04.1|Priority options Low/Normal/High/Critical|S-03 C-11|_[…]_|_[…]_|—|
|AC-04.2|лише Lead enabled|REF-01 + S-03|_[…]_|_[…]_|серверна авторизація|
|AC-05.1|Status всюди видно|S-02 + S-03|_[…]_|_[…]_|—|
|AC-05.2|Status на S-03|S-03|_[…]_|_[…]_|—|
|AC-05.3|Status у row|S-02 rows|_[…]_|_[…]_|—|
|AC-06.1|Empty comment → validation|S-03 comment validation|_[…]_|_[…]_|—|
|AC-06.2|Saved entry з'являється|S-03 comment saved|_[…]_|_[…]_|persistence|
|AC-06.3|Author saved|S-03 saved entry|_[…]_|_[…]_|—|
|AC-06.4|Timestamp saved|S-03 saved entry|_[…]_|_[…]_|—|
|AC-06.5|Save failure → error, no saved entry|S-03 failure|_[…]_|_[…]_|transactional save|
|AC-07.1|New = Open|S-03 new|_[…]_|_[…]_|—|
|AC-07.2|4 статуси|REF-01 enumeration|_[…]_|_[…]_|—|
|AC-07.3|Updated status зберігається|потрібен prototype flow|_[…]_|_[…]_|persistence|
|AC-07.4|Non-assignee не змінює status|REF-01 + S-03 Lead non-assignee|_[…]_|_[…]_|серверна авторизація|
|AC-07.5|Options обмежено дозволеними|S-03 Status dropdown|_[…]_|_[…]_|—|
|AC-08.1|ISS-001 (High/Open) у Important|S-02-B|_[…]_|_[…]_|—|
|AC-08.2|ISS-002 (Critical… або High) у Important|S-02-B|_[…]_|_[…]_|—|
|AC-08.3|ISS-005 (Normal) НЕ у Important|S-02-B|_[…]_|_[…]_|—|
|AC-08.4|ISS-004 (Closed/Critical) НЕ у Important|S-02-B|_[…]_|_[…]_|—|

## Окремі контрольні перевірки

- [ ] Team Lead non-assignee НЕ змінює status. _[Доказ]_: _[…]_
- [ ] У Create тільки Title/Description (не priority/status). _[Доказ]_: _[…]_
- [ ] High/Critical Resolved (ISS-003, ISS-007) у Important. _[Доказ]_: _[…]_
- [ ] Closed не має outgoing transitions. _[Доказ]_: _[…]_
- [ ] Усі джерельні ID реальні; немає US-10/US-11. _[Доказ]_: _[…]_
- [ ] HTTPS/backup/response time не перетворено на екрани. _[Доказ]_: _[…]_
- [ ] Немає зайвих controls: page icon placeholders, row-selection, bulk, Delete, Search, Sort, Attachments, AI features. _[Доказ]_: _[…]_

## Частина B — UX (до 5 доказових проблем)

|UX-ID|Проблема|Де видно|Рекомендація|
|---|---|---|---|
|UX-01|_[напр. labels обрізані у S-03]_|_[screenshot B]_|_[збільшити padding]_|
|UX-02|_[…]_|_[…]_|_[…]_|
|UX-03|_[…]_|_[…]_|_[…]_|

> Цей розділ не прирівнюється до сертифікації accessibility чи до тесту з реальними користувачами.

## Частина C — Defect / UX IDs

|DEF-ID / UX-ID|Severity|Location|Source|Evidence|Minimal correction|
|---|---|---|---|---|---|
|DEF-01|Major / Minor|S-03|AC-07.4|_[опис]_|_[опис]_|
|UX-01|Minor|M-01|design choice|_[опис]_|_[опис]_|

## Завершення

|Поле|Значення|
|---|---|
|UI design coverage|complete / partial / failed — _[обрати]_|
|Prototype evidence|verified by click-test / reported by student / not provided|
|Implementation tests outstanding|NFR-02 (HTTPS); NFR-03 (backup); NFR-04 (response time); persistence усіх даних; серверна авторизація|

> **Не давати загального «всі AC виконано», якщо є NOT VERIFIABLE.** Готувати перелік для людини; людина приймає/відхиляє зауваження.

## Контроль D — підпис студента

Студент приймає або відхиляє кожне зауваження з причиною. Після фінальних правок повторна перевірка хоча б вручну.

|Reviewer|Date|Accepted|Rejected|Comment|
|---|---|---|---|---|
|_[ПІБ]_|_[дата]_|_[перелік ID]_|_[перелік ID з причиною]_|_[…]_|

Якщо після останнього AI-аудиту макет змінено — зазначити які зміни AI вже не перевіряв.
