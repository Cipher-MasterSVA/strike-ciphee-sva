#Оглавление / Table of Contents

Русский:

1. Общее описание
2. Управление книгами
3. Создание книги
4. Защита книги (Виженер)
5. Шифрование текста
6. Дешифрование текста
7. Таймер безопасности (Web Worker)
8. Статистика и аудит книги
9. Двухкнижная схема
10. Настройки
11. Критические параметры безопасности
12. Защита от анализа трафика
13. Устойчивость к активному вмешательству (MITM)
14. Расширенные схемы управления книгами
15. Передача новых ключей через истощённую книгу
16. Ручной режим: шифрование на бумаге

English:

1. General Description
2. Book Management
3. Creating a Book
4. Book Protection (Vigenère)
5. Text Encryption
6. Text Decryption
7. Security Timer (Web Worker)
8. Book Statistics & Audit
9. Dual-Book Scheme
10. Settings
11. Critical Security Parameters
12. Traffic Analysis Protection
13. Resistance to Active Interference (MITM)
14. Advanced Book Management Schemes
15. Transmitting New Keys via Depleted Book
16. Manual Mode: Paper-Only Encryption

---

РУКОВОДСТВО ПОЛЬЗОВАТЕЛЯ (РУССКИЙ)

1. ОБЩЕЕ ОПИСАНИЕ

StrikeCipheeSVA — инструмент для шифрования текстов методом книжного шифра с пометкой использованных символов и удалением символа в ячейке (Amnesic Book Cipher — амнезия символов).

Программа представляет собой один HTML-файл, открывается в любом современном браузере, не требует установки и подключения к интернету. Все данные хранятся локально в IndexedDB браузера.

ОСНОВНОЙ ПРИНЦИП: Книга — трёхмерный массив символов (Страницы × Строки × Символы). При шифровании каждый символ сообщения заменяется координатами случайной свободной ячейки с таким же символом. Использованная ячейка немедленно УДАЛЯЕТСЯ — символ уничтожается, на его месте остаётся null. Без книги координаты выглядят как случайный набор чисел.

КЛЮЧЕВОЕ ОТЛИЧИЕ от классического книжного шифра: Использованные ячейки помечаются, после чего символ уничтожается. Книга не помнит, какие символы были использованы. После завершения сеанса связи невозможно доказать, какой символ был в ячейке. Это — «амнезия книги» (Amnesic Book). Даже при полном доступе к книге постфактум содержание переписки невосстановимо.

2. УПРАВЛЕНИЕ КНИГАМИ

ТОЧЕЧНОЕ УДАЛЕНИЕ КНИГИ: Выберите книгу из общего списка (обычные и защищённые) и нажмите «УДАЛИТЬ ВЫБРАННУЮ КНИГУ».

ПОЛНАЯ ОЧИСТКА БАЗЫ ДАННЫХ: Удаляет все книги из IndexedDB. Нажмите «УДАЛИТЬ ВСЕ КНИГИ» и подтвердите.

3. СОЗДАНИЕ КНИГИ

1. Введите имя книги.
2. Задайте размеры: Страницы, Строки, Символы в строке.
3. Выберите наборы символов: Кириллица, Латиница, Символы, Пробел.
4. Включите «Крипто-генератор» (crypto.getRandomValues).
5. Нажмите «СОЗДАТЬ КНИГУ».

ИМПОРТ И ЭКСПОРТ:
— Импорт обычной книги — выберите файл .json.
— Экспорт обычной книги — выберите книгу и нажмите «Экспорт обычной книги (JSON)».

ВАЖНО: После создания защищённой копии удалите открытую книгу через «Управление книгами» → «УДАЛИТЬ ВЫБРАННУЮ КНИГУ».

4. ЗАЩИТА КНИГИ (ВИЖЕНЕР)

ЗАШИФРОВАТЬ КНИГУ (отправитель):

1. Выберите режим «ЗАШИФРОВАТЬ КНИГУ».
2. Выберите книгу и нажмите «Загрузить».
3. Заполните: Алфавит, Ключ, Раунды (рекомендуется 17).
4. Нажмите «ВЫПОЛНИТЬ».
5. Выберите книгу из списка «Зашифрованная книга для экспорта» и нажмите «Экспорт».

РАСШИФРОВАТЬ КНИГУ (получатель):

1. Выберите режим «РАСШИФРОВАТЬ КНИГУ».
2. Нажмите «Импорт защищённой книги» и загрузите .json.
3. Выберите книгу и нажмите «Загрузить».
4. Введите те же Алфавит, Ключ и Раунды.
5. Нажмите «ВЫПОЛНИТЬ».

КРИТИЧЕСКИ ВАЖНО: Защищать можно только новую, ни разу не использованную книгу.

ПОСЛЕ СОЗДАНИЯ ЗАЩИЩЁННОЙ КНИГИ: удалите открытую книгу.

5. ШИФРОВАНИЕ ТЕКСТА

1. Выберите книгу и нажмите «Загрузить».
2. При загрузке введите алфавит, ключ и число раундов Виженера.
3. При первой загрузке введите 4-символьный пароль для продления таймера.
4. Введите сообщение.
5. Нажмите «ЗАШИФРОВАТЬ».
6. Результат — координаты вида страница:строка:символ.

РЕКОМЕНДАЦИЯ ПО ДЛИНЕ: Для защиты от анализа трафика используйте фиксированную длину сообщений (например, 40, 60 или 100 символов). Короткие сообщения добивайте мусором до стандартной длины.

ЗАЩИТА ОТ КЕЙЛОГЕРА: Включите «Виртуальная клавиатура». Раскладка случайна при каждом включении.

СКРЫТИЕ ТЕКСТА: Текст скрыт точками. Удерживайте «👁 Удерживать для просмотра», чтобы увидеть его.

6. ДЕШИФРОВАНИЕ ТЕКСТА

1. Выберите книгу и нажмите «Загрузить».
2. При загрузке введите алфавит, ключ и число раундов Виженера.
3. Вставьте координаты в поле «Шифр».
4. Нажмите «РАСШИФРОВАТЬ».
5. Результат скрыт точками. Удерживайте «👁 Удерживать для просмотра», чтобы увидеть его.

7. ТАЙМЕР БЕЗОПАСНОСТИ (Web Worker)

Единый таймер на 2 минуты на обе загруженные книги (для шифрования и дешифрования). Работает в фоновом Web Worker и НЕ приостанавливается при блокировке экрана. Используется реальное системное время.

Истечение таймера вызывает окно продления на 15 секунд. Нужно ввести пароль. При неверном пароле или бездействии все книги шифруются, поля очищаются.

8. СТАТИСТИКА И АУДИТ КНИГИ

1. Выберите книгу в блоке «Статистика».
2. Нажмите «Показать».
3. «Просмотреть книгу» — содержимое книги в отдельном окне. Символ [_] обозначает использованную (удалённую) ячейку.
4. «Резервная копия» — сохранить книгу в .json.

9. ДВУХКНИЖНАЯ СХЕМА

— Вы шифруете своей книгой (Книга-1). Собеседник расшифровывает её копией (Книга-1-копия).
— Собеседник шифрует своей книгой (Книга-2). Вы расшифровываете её копией (Книга-2-копия).

Таким образом, у каждого участника по две книги: одна для отправки, другая для получения.

10. НАСТРОЙКИ

— Язык интерфейса: Русский / English.
— Тема оформления: 🌙 тёмная / ☀️ светлая.
— Панель таймера: плавающая снизу.

11. КРИТИЧЕСКИЕ ПАРАМЕТРЫ БЕЗОПАСНОСТИ

11.1. ТРЕБОВАНИЯ К КНИГЕ

— «Крипто-генератор» должен быть включён (crypto.getRandomValues).
— Должны быть выбраны ВСЕ наборы символов.
— Размер книги подбирается под ожидаемую нагрузку.

Расчёт ёмкости (10 000 ячеек): При стандартной длине сообщения 40 символов — примерно 250 сообщений. 7 книг (по одной на день) — примерно на год общения.

11.2. ПРИНЦИП СВЕЖЕСТИ КНИГИ

Чем чаще заменяется книга — тем лучше. Это как вода в баке. Рекомендации: при малой нагрузке — раз в месяц, при средней — раз в неделю, при высокой — каждый день или каждый сеанс.

11.3. УТИЛИЗАЦИЯ ИСПОЛЬЗОВАННОЙ КНИГИ

Книга с дырами бесполезна. Как только перешли на новую — немедленно удалите старую. Противник, захвативший использованную книгу, не получает НИЧЕГО — только набор пустых ячеек.

11.4. ТРЕБОВАНИЯ К КЛЮЧУ ВИЖЕНЕРА

— Длина: не менее 40–60 символов.
— Ключ должен быть случайным (сгенерирован программой или бросанием костей).
— Хранить ключ отдельно от книги.

11.5. ТРЕБОВАНИЯ К АЛФАВИТУ ВИЖЕНЕРА

— Максимальная длина алфавита.
— Нажать «Перемешать» для изменения порядка символов.
— Получатель должен использовать ТОЧНО такой же алфавит.

12. ЗАЩИТА ОТ АНАЛИЗА ТРАФИКА

1. Договоритесь с собеседником о СТАНДАРТНОЙ ДЛИНЕ сообщений (40, 60 или 100 символов).
2. Короткие сообщения добивайте случайным мусором до стандартной длины.
3. При расшифровке читайте начало сообщения, игнорируйте хвост из мусора.
4. Счётчик символов в интерфейсе помогает контролировать длину.

13. УСТОЙЧИВОСТЬ К АКТИВНОМУ ВМЕШАТЕЛЬСТВУ (MITM)

Если противник перехватит и изменит координаты в пути, получатель при расшифровке увидит КАШУ (бессмысленный набор символов) вместо текста. Это сразу обнаруживается. Противник НЕ МОЖЕТ незаметно подменить сообщение. Содержание переписки остаётся защищённым.

14. РАСШИРЕННЫЕ СХЕМЫ УПРАВЛЕНИЯ КНИГАМИ

14.1. БАЗОВАЯ РОТАЦИЯ: ОДНА КНИГА В ДЕНЬ

7 комплектов книг (по одному на каждый день недели). Каждый день используется своя книга. Компрометация одного дня раскрывает только этот день.

14.2. ПРОДВИНУТАЯ РОТАЦИЯ: КОМПЛЕКТ НА КАЖДЫЙ СЕАНС

N комплектов книг. Один сеанс связи — один комплект. После сеанса комплект немедленно удаляется.

15. ПЕРЕДАЧА НОВЫХ КЛЮЧЕЙ ЧЕРЕЗ ИСТОЩЁННУЮ КНИГУ

Когда текущая книга близка к истощению, новые параметры (новый ключ Виженера, параметры книги или сами координаты новой книги) передаются в шифрованном сообщении через эту же книгу.

Поскольку сообщение защищено истощающейся книгой (аналог одноразового блокнота), перехват не даёт противнику ничего.

Первая личная встреча (или закладка) нужна только ОДИН РАЗ — для передачи самой первой книги. Далее система полностью самоподдерживается.

16. РУЧНОЙ РЕЖИМ: ШИФРОВАНИЕ НА БУМАГЕ

16.1. КОГДА ЭТО НУЖНО

— Нет электричества или устройств.
— Требуется полная изоляция от цифрового следа.

16.2. ПОДГОТОВКА КНИГИ ДЛЯ ПЕЧАТИ

Распечатайте книгу. Создайте ДВА экземпляра. Передайте получателю.

СПОСОБЫ ПЕРЕДАЧИ:
— Личная встреча (наиболее надёжный).
— Закладка (наиболее анонимный).
— Доверенный курьер (запасной вариант).

ВАЖНО: Первая передача книги — самая безопасная. Слежки за вами ещё нет.

16.3. ШИФРОВАНИЕ ВРУЧНУЮ

Для каждого символа сообщения:

1. Найдите ПЕРВУЮ свободную ячейку с таким символом (по порядку: страницы, строки, символы).
2. Запишите её координаты (страница:строка:символ).
3. Зачеркните использованную ячейку (уничтожьте символ).
4. Добавьте мусорные символы до стандартной длины сообщения.

16.4. ДЕШИФРОВАНИЕ ВРУЧНУЮ

Для каждой полученной координаты:

1. Найдите ячейку по координатам.
2. Прочитайте и запишите символ.
3. Зачеркните ячейку (уничтожьте символ).

16.5. СИНХРОНИЗАЦИЯ

Отправитель всегда ищет ПЕРВУЮ свободную ячейку по порядку (а не случайную, как в цифровом режиме). Это гарантирует, что книги отправителя и получателя останутся синхронными.

16.6. ГЕНЕРАЦИЯ КНИГИ БЕЗ КОМПЬЮТЕРА

— Способ 1: Игральные кости (для генерации случайных символов).
— Способ 2: Мешок с жетонами (символы на жетонах, вытягивание вслепую).
— Способ 3: Таблица случайных чисел (заранее подготовленная или взятая из книги).

16.7. ОСОБЕННОСТИ РУЧНОГО РЕЖИМА

— Шифр Виженера не используется (книга существует только на бумаге).
— Цифровой след отсутствует полностью.
— Криптостойкость равна цифровому режиму (та же амнезия символов).

16.8. УНИЧТОЖЕНИЕ

Использованную книгу — сжечь до пепла. Пепел развеять. Не оставлять восстановимых следов.

16.9. ЯЗЫКОВАЯ УНИВЕРСАЛЬНОСТЬ КНИГИ

Русская «А» и английская «A» — один и тот же звук. Книга не привязана к конкретному языку. Любой язык можно записать через доступные в книге символы (транслитерация).

16.10. НЕВОЗМОЖНОСТЬ ДОКАЗАТЕЛЬСТВА

Даже перебрав все возможные варианты заполнения пустых ячеек, противник получит тысячи осмысленных сообщений. Все они равновероятны. Доказать, какое из них было настоящим — невозможно. Информация уничтожена безвозвратно. Никакой силой. Никаким компьютером. Никогда.

---

USER MANUAL (ENGLISH)

1. GENERAL DESCRIPTION

StrikeCipheeSVA is a text encryption tool based on the book cipher method with marking of used cells and deletion of the symbol in the cell (Amnesic Book Cipher — symbol amnesia).

The program is a single HTML file. It opens in any modern browser, requires no installation and no internet connection. All data is stored locally in the browser's IndexedDB.

CORE PRINCIPLE: The Book is a three-dimensional array of symbols (Pages × Rows × Characters). During encryption, each character of the message is replaced by the coordinates of a random free cell containing the same character. The used cell is immediately DELETED — the symbol is destroyed, and null remains in its place. Without the book, the coordinates appear as a random set of numbers.

KEY DIFFERENCE from the classical book cipher: Used cells are marked, and then the symbol is destroyed. The book does NOT remember which symbols were used. After the communication session is over, it is impossible to prove which symbol was in any given cell. This is the "Amnesia of the Book" (Amnesic Book). Even with full access to the book post factum, the content of the correspondence cannot be recovered.

2. BOOK MANAGEMENT

TARGETED DELETION: Select a book from the general list (regular and protected) and click "DELETE SELECTED BOOK".

COMPLETE DATABASE PURGE: Deletes all books from IndexedDB. Click "DELETE ALL BOOKS" and confirm.

3. CREATING A BOOK

1. Enter a book name.
2. Set dimensions: Pages, Rows, Characters per row.
3. Select character sets: Cyrillic, Latin, Symbols, Space.
4. Enable "Crypto-generator" (uses crypto.getRandomValues).
5. Click "CREATE BOOK".

IMPORT AND EXPORT:
— Import a regular book — select a .json file.
— Export a regular book — select a book and click "Export regular book (JSON)".

IMPORTANT: After creating a protected copy, delete the open book via "Book Management" → "DELETE SELECTED BOOK".

4. BOOK PROTECTION (VIGENÈRE)

ENCRYPT A BOOK (sender):

1. Select the "ENCRYPT BOOK" mode.
2. Select a book and click "Load".
3. Fill in: Alphabet, Key, Rounds (17 is recommended).
4. Click "EXECUTE".
5. Select the book from the "Encrypted book for export" list and click "Export".

DECRYPT A BOOK (recipient):

1. Select the "DECRYPT BOOK" mode.
2. Click "Import protected book" and load the .json file.
3. Select the book and click "Load".
4. Enter the exact same Alphabet, Key, and Rounds.
5. Click "EXECUTE".

CRITICALLY IMPORTANT: Only protect a new, never-used book.

AFTER CREATING A PROTECTED BOOK: delete the open (unprotected) book.

5. TEXT ENCRYPTION

1. Select a book and click "Load".
2. When loading, enter the Vigenère alphabet, key, and number of rounds.
3. On the first load, enter a 4-character password for extending the timer.
4. Type your message.
5. Click "ENCRYPT".
6. The result is a set of coordinates in the format page:row:character.

LENGTH RECOMMENDATION: To protect against traffic analysis, use a fixed message length (e.g., 40, 60, or 100 characters). Pad short messages with random junk characters to the standard length.

KEYLOGGER PROTECTION: Enable "Virtual Keyboard". The layout is randomized each time it is activated.

TEXT HIDING: The text is hidden behind dots. Hold "👁 Hold to reveal" to view it.

6. TEXT DECRYPTION

1. Select a book and click "Load".
2. When loading, enter the Vigenère alphabet, key, and number of rounds.
3. Paste the received coordinates into the "Cipher" field.
4. Click "DECRYPT".
5. The result is hidden behind dots. Hold "👁 Hold to reveal" to view it.

7. SECURITY TIMER (Web Worker)

A single 2-minute timer for both loaded books (encryption and decryption). It runs in a background Web Worker and does NOT pause when the screen is locked. It uses real system time.

When the timer expires, a 15-second extension window appears. The password must be entered. If the password is incorrect or no action is taken, all books are encrypted and all fields are cleared.

8. BOOK STATISTICS & AUDIT

1. Select a book in the "Statistics" block.
2. Click "Show".
3. "View Book" — displays the book content in a separate window. The symbol [_] denotes a used (deleted) cell.
4. "Backup Copy" — save the book as a .json file.

9. DUAL-BOOK SCHEME

— You encrypt with your book (Book-1). Your contact decrypts with its copy (Book-1-copy).
— Your contact encrypts with their book (Book-2). You decrypt with its copy (Book-2-copy).

Thus, each participant has two books: one for sending, one for receiving.

10. SETTINGS

— Interface language: Русский / English.
— Theme: 🌙 dark / ☀️ light.
— Timer panel: floating at the bottom.

11. CRITICAL SECURITY PARAMETERS

11.1. BOOK REQUIREMENTS

— "Crypto-generator" must be enabled (uses crypto.getRandomValues).
— ALL character sets must be selected.
— Book size should be chosen according to the expected communication load.

Capacity calculation (10,000 cells): With a standard message length of 40 characters — approximately 250 messages. 7 books (one per day) — roughly a year of communication.

11.2. BOOK FRESHNESS PRINCIPLE

The more frequently a book is replaced, the better. Think of it like water in a tank. Recommendations: light usage — once a month, medium usage — once a week, heavy usage — every day or every session.

11.3. DISPOSAL OF USED BOOKS

A book with holes is useless. As soon as you switch to a new one, immediately delete the old one. An adversary who seizes a used book gets NOTHING — only a set of empty cells.

11.4. VIGENÈRE KEY REQUIREMENTS

— Length: at least 40–60 characters.
— The key must be random (generated by the program or by throwing dice).
— Store the key separately from the book.

11.5. VIGENÈRE ALPHABET REQUIREMENTS

— Maximum alphabet length.
— Click "Shuffle" to randomize the symbol order.
— The recipient must use the EXACT same alphabet.

12. TRAFFIC ANALYSIS PROTECTION

1. Agree with your contact on a STANDARD message length (40, 60, or 100 characters).
2. Pad short messages with random junk to the standard length.
3. When decrypting, read the beginning of the message; ignore the junk tail.
4. The character counter in the interface helps control the length.

13. RESISTANCE TO ACTIVE INTERFERENCE (MITM)

If an adversary intercepts and alters the coordinates in transit, the recipient will see GARBAGE (a meaningless string of characters) upon decryption, instead of the text. This is detected immediately. The adversary CANNOT secretly substitute the message. The content of the correspondence remains protected.

14. ADVANCED BOOK MANAGEMENT SCHEMES

14.1. BASIC ROTATION: ONE BOOK PER DAY

7 book sets (one for each day of the week). Each day has its own book. Compromise of one day reveals only that day's communication.

14.2. ADVANCED ROTATION: ONE SET PER SESSION

N book sets. One communication session — one set. After the session, the set is immediately deleted.

15. TRANSMITTING NEW KEYS VIA A DEPLETED BOOK

When the current book is nearly exhausted, new parameters (a new Vigenère key, book parameters, or the coordinates of a new book) are transmitted in an encrypted message through this same book.

Since the message is protected by the depleting book (analogous to a one-time pad), interception gives the adversary nothing.

The initial in-person meeting (or dead drop) is only needed ONCE — for the transfer of the very first book. After that, the system is fully self-sustaining.

16. MANUAL MODE: PAPER-ONLY ENCRYPTION

16.1. WHEN THIS IS NEEDED

— No electricity or devices available.
— Complete isolation from any digital footprint is required.

16.2. PREPARING A BOOK FOR PRINTING

Print the book. Make TWO copies. Transfer one copy to the recipient.

TRANSFER METHODS:
— In-person meeting (most reliable).
— Dead drop (most anonymous).
— Trusted courier (backup option).

IMPORTANT: The first transfer of the book is the safest. No surveillance is on you yet.

16.3. ENCRYPTING BY HAND

For each character of the message:

1. Find the FIRST free cell with that character (in order: pages, rows, characters).
2. Write down its coordinates (page:row:character).
3. Cross out the used cell (destroy the symbol).
4. Add junk characters to reach the standard message length.

16.4. DECRYPTING BY HAND

For each received coordinate:

1. Find the cell by its coordinates.
2. Read and write down the character.
3. Cross out the cell (destroy the symbol).

16.5. SYNCHRONIZATION

The sender always searches for the FIRST free cell in order (not random, as in digital mode). This guarantees that the sender's and recipient's books remain perfectly synchronized.

16.6. GENERATING A BOOK WITHOUT A COMPUTER

— Method 1: Dice (for generating random characters).
— Method 2: A bag of tokens (characters on tokens, drawn blindly).
— Method 3: A random number table (prepared in advance or taken from a book).

16.7. CHARACTERISTICS OF MANUAL MODE

— Vigenère cipher is not used (the book exists only on paper).
— Digital footprint is completely absent.
— Cryptographic strength is equal to the digital mode (same symbol amnesia).

16.8. DESTRUCTION

Burn the used book. To ashes. Scatter the ashes. Leave no recoverable traces.

16.9. LANGUAGE UNIVERSALITY OF THE BOOK

The Russian "А" and the English "A" are the same sound. The book is not tied to a specific language. Any language can be written using the symbols available in the book (via transliteration). The book is universal.

16.10. IMPOSSIBILITY OF PROOF

Even after trying every possible combination to fill the empty cells, an adversary will obtain thousands of meaningful messages. All of them are equally probable. It is impossible to prove which one was the real message. The information is destroyed. Irreversibly. By any power. By any computer. Ever.

---

Лицензия / License

MIT License © Cipher_MasterSVA, 2026.
