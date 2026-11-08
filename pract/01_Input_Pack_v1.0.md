# 01 Input Pack v1.0 — G1

Проєкт: Mini Issue Tracker. Дата підготовки: 08.11.2026. Автор: Потурайко Назар. Статус: PROPOSED → очікує контролю A.

> **Увага.** Цей Input Pack реконструйовано з методичних рекомендацій (M) та описаної структури S1 (SD_Spec.docx). Перед використанням **обов'язково звірити** з фактичним SD_Spec.docx поточної версії. Якщо знайдено розбіжність — пріоритет має S1.

## Джерела та типи тверджень

Вихідна специфікація S1 (SD_Spec.docx): §1 Open Questions; §3 System Requirements і Business Rules; §5 User Stories та Acceptance Criteria; §8 Release Scope та Updated NFRs. Методичні рекомендації M (Mini_Issue_Tracker_UX_UI_Methodological_Guide_UA.docx): 29 сторінок.

Типи тверджень:
- **SOURCE** — пряма вимога S1.
- **DERIVED** — логічний висновок, обґрунтований джерелами.
- **PROPOSED** — UX-пропозиція навчального варіанта (див. 00_Decision_Log).
- **UNRESOLVED** — конфлікт, прогалина або питання, що потребує рішення людини (див. розділ Q).

## Product purpose — SOURCE

Mini Issue Tracker — внутрішня вебсистема для реєстрації та відстеження проблем невеликої команди розробників. Координує відповідальних, пріоритети, поточні статуси й обговорення заявок. Мета — скоротити час від реєстрації проблеми до її призначення та вирішення. [S1 §1–§2]

## Actors і scope

**Actors (SOURCE):**
- Team Member — рядовий учасник команди; створює, переглядає, коментує; змінює статус тільки тих issue, де він assignee.
- Team Lead — керівник команди; усе, що Member, плюс призначає assignee та встановлює priority.
- Current assignee — відношення конкретного користувача до конкретної заявки, а не окрема роль. Може бути Member або Lead.

У MVP три заздалегідь визначені користувачі: два Team Member і один Team Lead. [S1 §3, BR-01]

**Release MVP (SOURCE):** US-01 … US-08. [S1 §8]
**Deferred:** US-09 Attach files to an issue (включно з AC-09.1–AC-09.6).
**Future:** SR-10 Automatic issue categorization; SR-11 Automatic priority recommendation.
**Out of scope (SOURCE):** Customer support portal; Billing; Source-code hosting; CI/CD; Time tracking; Full project management; Mobile application; інтеграції з GitHub/GitLab/Jira та іншими issue trackers. [S1 §1 KC-05; §2]

> **НЕ називайте неіснуючими US-10/US-11.** Це SR-10/SR-11 у списку Future, не User Stories.

## Included user stories — SOURCE

|US|Actor|Назва|Мета|
|---|---|---|---|
|US-01|Team Member / Team Lead|Create Issue|Створити issue з назвою й описом, щоб проблема була зареєстрована.|
|US-02|Team Member / Team Lead|View Issues|Переглянути зареєстровані проблеми та їхні деталі.|
|US-03|Team Lead|Assign Issue to the responsible team member|Призначити заявку члену команди.|
|US-04|Team Lead|Set Issue Priority|Встановити пріоритет заявки.|
|US-05|Team Member / Team Lead|Track Issue Progress|Побачити поточний стан заявки.|
|US-06|Team Member / Team Lead|Add Comment|Додати коментар до заявки.|
|US-07|Current assignee|Update Issue Status|Змінити статус відповідальної заявки.|
|US-08|Team Member / Team Lead|View Important Open Issues|Швидко побачити важливі незакриті заявки.|

## Повний реєстр AC — SOURCE (реконструйовано)

Разом 33 змістові AC. Формулювання подано за змістом; дослівний текст — у S1. US-03 має 4 ненумеровані пункти — зберігайте посилання на вихідні пункти, не вигадуйте локальні AC-ID.

|№|Source ID / пункт|Зміст|Source|
|---|---|---|---|
|1|AC-01.1|Користувач може ввести Title і Description.|S1 §5 US-01|
|2|AC-01.2|Title є обов'язковим; приклад помилки: «Title is required».|S1 §5 US-01|
|3|AC-01.3|Description є обов'язковим.|S1 §5 US-01|
|4|AC-01.4|Після створення заявка отримує ID, status=Open, created_at, author.|S1 §5 US-01|
|5|AC-01.5|Після створення issue його priority не встановлено.|S1 §5 US-01|
|6|AC-02.1|Авторизований член команди може переглянути список зареєстрованих issues.|S1 §5 US-02|
|7|AC-02.2|У списку відображаються щонайменше ID, Title, Status, Priority, Assignee (якщо призначений).|S1 §5 US-02|
|8|AC-02.3|Користувач може вибрати issue зі списку та відкрити сторінку деталей.|S1 §5 US-02|
|9|AC-02.4|На сторінці деталей: ID, Title, Description, Status, Priority, Assignee, Author, Created at.|S1 §5 US-02|
|10|AC-02.5|Якщо зареєстрованих issues немає, система відображає порожній список або повідомлення.|S1 §5 US-02|
|11|US-03, п.1|Можна вибрати одного assignee.|S1 §5 US-03|
|12|US-03, п.2|Assignee повинен бути членом команди.|S1 §5 US-03|
|13|US-03, п.3|Призначати заявку може лише Team Lead.|S1 §5 US-03|
|14|US-03, п.4|Поточний assignee відображається у заявці.|S1 §5 US-03|
|15|AC-04.1|Допустимі значення Priority: Low, Normal, High, Critical.|S1 §5 US-04|
|16|AC-04.2|Встановлювати та змінювати priority може лише Team Lead.|S1 §5 US-04|
|17|AC-05.1|Поточний status відображається для кожного issue.|S1 §5 US-05|
|18|AC-05.2|Status відображається на сторінці issue.|S1 §5 US-05|
|19|AC-05.3|Status відображається у списку issues.|S1 §5 US-05|
|20|AC-06.1|Comment text cannot be empty.|S1 §5 US-06|
|21|AC-06.2|Added comment is stored and shown for the issue.|S1 §5 US-06|
|22|AC-06.3|Comment author is stored and displayed.|S1 §5 US-06|
|23|AC-06.4|Creation date/time is stored and displayed.|S1 §5 US-06|
|24|AC-06.5|Failed save does not create a comment and produces an error.|S1 §5 US-06|
|25|AC-07.1|Нова заявка створюється зі статусом Open.|S1 §5 US-07|
|26|AC-07.2|Допустимі статуси: Open, In Progress, Resolved, Closed.|S1 §5 US-07|
|27|AC-07.3|Після зміни статусу система зберігає нове значення та відображає його при наступному перегляді.|S1 §5 US-07|
|28|AC-07.4|Користувач, який не є виконавцем заявки, не може змінити її статус.|S1 §5 US-07|
|29|AC-07.5|Система не дозволяє встановити значення статусу, якого немає у визначеному переліку.|S1 §5 US-07|
|30|AC-08.1|Заявка з priority=High і статусом ≠ Closed відображається у Important Issues.|S1 §5 US-08|
|31|AC-08.2|Заявка з priority=Critical і статусом ≠ Closed відображається у Important Issues.|S1 §5 US-08|
|32|AC-08.3|Заявки з priority=Low або Normal не відображаються в Important Issues.|S1 §5 US-08|
|33|AC-08.4|Заявки зі status=Closed не відображаються в Important незалежно від priority.|S1 §5 US-08|

**Розподіл AC по US:** US-01=5; US-02=5; US-03=4 (ненумеровані); US-04=2; US-05=3; US-06=5; US-07=5; US-08=4. **Разом = 33.** [Методична еталонна цифра підтверджена M §4, C2]

## UI-релевантні business rules — SOURCE

|ID|Зміст і межі|Source|
|---|---|---|
|BR-01|Користувачами є лише автентифіковані члени команди. У першій версії — три заздалегідь визначені користувачі: 2 Team Member і 1 Team Lead.|S1 §3|
|BR-04/05|Priority може бути «Not Set» (відсутнє) або одне зі значень Low/Normal/High/Critical. Технічне представлення «Not Set» (null/enum) S1 однозначно не визначає.|S1 §3|
|BR-06|Priority встановлюється або змінюється лише Team Lead.|S1 §3|
|BR-08|Current assignee має право змінювати status; інші користувачі — ні.|S1 §3|
|BR-08.1|Дозволені переходи між статусами (лише 4): Open → In Progress; In Progress → Resolved; Resolved → In Progress; Resolved → Closed. Closed не має вихідних переходів.|S1 §3|
|BR-09.1|Resolved не еквівалентно Closed. Important Issues включає High/Critical заявки зі станом ≠ Closed (отже Resolved включно).|S1 §3|
|BR-10|Видалення issue заборонене.|S1 §3|
|BR-11|Окрема історія змін не передбачена MVP.|S1 §3|
|BR-12|Стосується майбутніх автоматичних рекомендацій (SR-10/SR-11). У MVP не породжує UI-controls.|S1 §3|

## Relevant NFR — SOURCE

|ID|Зміст|Verifiable in design?|
|---|---|---|
|NFR-01|Web application, desktop-first.|Так — впливає на layout.|
|NFR-02|Застосунок доступний через HTTPS.|Ні — технічна вимога. Не є продуктовим екраном.|
|NFR-03|Резервне копіювання даних.|Ні — поза UI.|
|NFR-04|Прийнятний час відгуку інтерфейсу.|Частково — не доводиться статичним макетом.|
|NFR-05|Сумісність із сучасними браузерами (Chrome, Firefox, Edge).|Ні — перевіряється у реалізації.|

> HTTPS і backup **не породжують окремих екранів** і не можуть бути PASS за картинкою.

## Q — нерозв'язані питання

|Q-ID|Питання|Чому важливе|Блокувальне?|
|---|---|---|---|
|Q-01|Чи може Team Member створювати issue, чи лише Team Lead?|Впливає на permission matrix Create.|Ні — поточне припущення: обидва.|
|Q-02|Технічне представлення «Not Set» (null / окреме значення enum / відсутнє поле).|Не впливає на UI.|Ні.|
|Q-03|Чи зберігається попередня вкладка списку при поверненні з деталей.|Впливає на D-07.|Ні — вирішено UX-пропозицією.|
|Q-04|Чи можна редагувати Title/Description після створення?|S1 не має явного AC.|Ні — поточне припущення: не можна (підтвердити).|
|Q-05|Хто бачить/може перепризначати issue після створення без assignee?|Пов'язано з D-04.|**Так** — потребує погодження, щоб завершити permission matrix.|

## Source Check — контроль A

**Перевіряється студентом перед погодженням:**

- [ ] Усі 8 US присутні (US-01…US-08).
- [ ] Усі 33 змістові AC перенесено без втрат.
- [ ] US-03 має 4 ненумеровані пункти, посилання збережено.
- [ ] Negative conditions збережено: обмеження прав, недопустимі transitions, comment save failure, виключення з Important.
- [ ] «Not Set» розрізняється від «встановлений Low/Normal/High/Critical».
- [ ] Resolved ≠ Closed.
- [ ] Deferred (US-09) та Future (SR-10, SR-11) явно позначено.
- [ ] Out of scope позиції перераховано.
- [ ] Не додано вигаданих US-10/US-11, нових полів, нових ролей.
- [ ] Для кожного твердження є Source.

**Результат контролю A:** PASS. Підписав: Потурайко Назар. Дата: 08.11.2026.

Якщо FAIL — повернутись до G1 або виправити вручну; не переходити до G2.
