# 07 Prototype P1 — протокол проходження (ШАБЛОН)

Проєкт: Mini Issue Tracker. Дата проходження: _[дд.мм.рррр]_. Оператор: _[ПІБ]_. Робоча версія Figma: _[назва/дата]_. Статус: заповнюється під час Present.

**Figma file:** _[https://www.figma.com/design/...]_
**Prototype link (Present):** _[https://www.figma.com/proto/...]_

> Тестові дані фіктивні. Прототип симулює підготовлені стани, без backend і довільного введення.

## Структура Flows (18 стартових точок)

### Основні

|Flow|Starting frame|Node ID|Сценарій|
|---|---|---|---|
|Flow 1 — основний|S-02-A|_[node-id]_|All → Important → Title → Details → Return. Create → M-01 → заповнити → Create → S-03 нової issue → Return → список з новою.|
|P1 — Zero issues|S-02-C|_[node-id]_|No issues yet → Create → форма → ISS-009 → Return → список з однією.|
|Flow 2 — з All|M-01 valid-from-All|_[node-id]_|Create → ISS-009 → Return → All з 9 записами.|
|Flow 3 — з Important|M-01 valid-from-Important|_[node-id]_|Create → ISS-009 (Not Set) → Return → Important залишається 4.|
|Flow 4 — з Empty|M-01 valid-from-Empty|_[node-id]_|Create → ISS-009 → Return → All з 1 записом.|

### Assign / Priority

|Flow|Сценарій|Очікування|
|---|---|---|
|Flow 5|Lead → ISS-005 (Unassigned) → Assign Member A → Apply|Assignee оновлено на Member A|
|Flow 6|Lead → ISS-002 → Reassign Member B|Assignee оновлено|
|Flow 7|Lead → ISS-006 → Priority Low → Apply|Priority = Low|
|Flow 8|Lead → ISS-006 → Priority Normal → Apply|Priority = Normal|
|Flow 9|Lead → ISS-005 → Priority High → Apply|Priority = High (з'являється в Important)|
|Flow 10|Lead → ISS-008 → Priority Critical → Apply|Priority = Critical|

### Status transitions (валідні)

|Flow|Сценарій|Очікування|
|---|---|---|
|Flow 11|Member A (assignee ISS-001 Open) → Status → In Progress → Apply|Status оновлено|
|Flow 12|Member B (assignee ISS-002 In Progress) → Status → Resolved → Apply|Status оновлено|
|Flow 13|Member A (assignee ISS-003 Resolved) → Status → In Progress → Apply|Повернення в роботу|
|Flow 14|Member A (assignee ISS-003 Resolved) → Status → Closed → Apply|У Important зникає; в All залишається з Closed|

### Comments

|Flow|Сценарій|Очікування|
|---|---|---|
|Flow 15|Порожній коментар → Add Comment|validation error; saved entry не додано|
|Flow 16|Заповнений → Add Comment → success|saved entry з author/time|
|Flow 17|Заповнений → Add Comment → save failure|error-banner; saved entry не додано|

## Детальний протокол connections

> Для кожного connection у Figma Prototype tab: Trigger (On click / On tap), Action (Navigate to / Open overlay / Close overlay), Destination.

|P-ID|From frame|Trigger|Action|Destination|Status|Коментар|
|---|---|---|---|---|---|---|
|P-01|S-02-A header Create|On click|Open overlay|M-01|CONFIGURED / PASS|—|
|P-02|M-01 Cancel|On click|Close overlay|(back to S-02-A)|CONFIGURED / PASS|—|
|P-03|M-01 Create (invalid-Title state)|On click|Navigate|M-01 validation-Title|CONFIGURED / PASS|—|
|P-04|M-01 Create (invalid-Description)|On click|Navigate|M-01 validation-Description|CONFIGURED / PASS|—|
|P-05|M-01 Create (valid)|On click|Navigate|S-03 ISS-009|CONFIGURED / PASS|prepared stub|
|P-06|S-03 Return|On click|Navigate|S-02-A after-create|CONFIGURED / PASS|—|
|P-07|S-02-A Tab Important|On click|Navigate|S-02-B|CONFIGURED / PASS|—|
|P-08|S-02-B Tab All|On click|Navigate|S-02-A|CONFIGURED / PASS|—|
|P-09|S-02-A row ISS-001 Title|On click|Navigate|S-03 ISS-001|CONFIGURED|_[перевірити]_|
|P-10|S-02-A row ISS-002 Title|On click|Navigate|S-03 ISS-002|CONFIGURED|_[…]_|
|_[…]_|_[…]_|_[…]_|_[…]_|_[…]_|_[…]_|_[…]_|
|P-17|S-03 ISS-003 Status dropdown → Closed → Apply|On click|Navigate|S-03 ISS-003 Closed|_[…]_|—|
|P-18|S-03 ISS-003 Closed Return|On click|Navigate|S-02-B (after-close) — ISS-003 зникає|_[…]_|—|

**Статус:**
- CONFIGURED — зв'язок налаштовано у редакторі.
- PASS — click-test у Present пройдено, поведінка як очікувано.
- NOT RUN — click-test не виконано.
- FAIL — результат не відповідає очікуванню.

## Підсумок виконання

|Показник|Значення|
|---|---|
|Усього configured connections|_[N]_|
|PASS (пройдено у Present)|_[M]_|
|NOT RUN|_[K]_|
|FAIL|_[F]_|
|Дата проходження|_[дата]_|

> Повний повтор кожного клікабельного Title не обов'язковий. CONFIGURED означає оглянуту interaction у редакторі; NOT RUN — відсутність click-test, не доведений дефект.

## Мінітест з одногрупником

Дайте три завдання без підказок:

|Завдання|Очікуваний маршрут|Факт|Проблеми|
|---|---|---|---|
|1. Створити заявку «Тестова проблема»|S-02 → Create → M-01 → Title+Description → Create → S-03|_[опис проходження]_|_[що викликало вагання]_|
|2. Знайти важливу Resolved-заявку|S-02 → Important → побачити ISS-003 / ISS-007|_[…]_|_[…]_|
|3. Пояснити чому Member B не може змінити status ISS-002 (assignee = Member A)|Відкрити ISS-002 → Status disabled|_[…]_|_[…]_|

**Висновок:** _[що зрозуміле / що неясне / що пропонується поправити]_.

> AI може допомогти з класифікацією проблем, але не замінює фактичні дії людини в прототипі. Це спостереження конкретного проходження, не доказ універсальної зручності.

## Чесна симуляція — записи

- Сценарій починається з S-02 для заданого користувача; S-01 не симулює пароль чи SSO.
- Для введення Title/Description у M-01 використовується підготовлений заповнений стан; результат — prepared ISS-009, не збереження у БД.
- Статуси і permissions перевіряються за REF-01 та підготовленими state examples, не за conditional logic у прототипі.
- Намальована стрілка в Review notes НЕ є створеним prototype connection.
