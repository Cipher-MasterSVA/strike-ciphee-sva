# StrikeCipheeSVA — Amnesic Book Cipher

Абсолютно стойкий книжный шифр с амнезией (Amnesic Book Cipher).
Удовлетворяет условиям OTP (теорема Шеннона, 1949).
Один HTML-файл. Браузер или бумага.
Без интернета. Без сервера. Без зависимостей.

---

## Содержание

- [Руководство пользователя (Русский)](#руководство-пользователя-русский)
- [User Manual (English)](#user-manual-english)

---

# РУКОВОДСТВО ПОЛЬЗОВАТЕЛЯ (РУССКИЙ)

## 1. ОБЩЕЕ ОПИСАНИЕ

StrikeCipherSVA — инструмент для шифрования текстов методом книжного шифра
с пометкой использованных символов и удалением символа в ячейке
(Amnesic Book Cipher — амнезия символов).

Программа представляет собой один HTML-файл, открывается в любом современном
браузере, не требует установки и подключения к интернету. Все данные хранятся
локально в IndexedDB браузера.

**ОСНОВНОЙ ПРИНЦИП:**
Книга — трёхмерный массив символов (Страницы × Строки × Символы).
При шифровании каждый символ сообщения заменяется координатами случайной
свободной ячейки с таким же символом. Использованная ячейка немедленно
УДАЛЯЕТСЯ — символ уничтожается, на его месте остаётся null. Без книги
координаты выглядят как случайный набор чисел.

**КЛЮЧЕВОЕ ОТЛИЧИЕ от классического книжного шифра:**
Использованные ячейки помечаются, после чего символ уничтожается. Книга
не помнит, какие символы были использованы. После завершения сеанса связи
невозможно доказать, какой символ был в ячейке. Это — «амнезия книги»
(Amnesic Book). Даже при полном доступе к книге постфактум содержание
переписки невосстановимо.

---

## 2. УПРАВЛЕНИЕ КНИГАМИ

**ТОЧЕЧНОЕ УДАЛЕНИЕ КНИГИ:**
Выберите книгу из общего списка (обычные и защищённые) и нажмите
«УДАЛИТЬ ВЫБРАННУЮ КНИГУ».

**ПОЛНАЯ ОЧИСТКА БАЗЫ ДАННЫХ:**
Удаляет все книги из IndexedDB. Нажмите «УДАЛИТЬ ВСЕ КНИГИ» и подтвердите.

---

## 3. СОЗДАНИЕ КНИГИ

1. Введите имя книги.
2. Задайте размеры: Страницы, Строки, Символы в строке.
3. Выберите наборы символов: Кириллица, Латиница, Символы, Пробел.
4. Включите «Крипто-генератор» (crypto.getRandomValues).
5. Нажмите «СОЗДАТЬ КНИГУ».

**ИМПОРТ И ЭКСПОРТ:**
— Импорт обычной книги — выберите файл .json.
— Экспорт обычной книги — выберите книгу и нажмите «Экспорт обычной книги (JSON)».

**ВАЖНО:** После создания защищённой копии удалите открытую книгу через
«Управление книгами» → «УДАЛИТЬ ВЫБРАННУЮ КНИГУ».

---

## 4. ЗАЩИТА КНИГИ (ВИЖЕНЕР)

**ЗАШИФРОВАТЬ КНИГУ (отправитель):**
1. Выберите режим «ЗАШИФРОВАТЬ КНИГУ».
2. Выберите книгу и нажмите «Загрузить».
3. Заполните: Алфавит, Ключ, Раунды (рекомендуется 17).
4. Нажмите «ВЫПОЛНИТЬ».

**ЭКСПОРТ ЗАШИФРОВАННОЙ КНИГИ:**
Выберите книгу из списка «Зашифрованная книга для экспорта» и нажмите «Экспорт».

**РАСШИФРОВАТЬ КНИГУ (получатель):**
1. Выберите режим «РАСШИФРОВАТЬ КНИГУ».
2. Нажмите «Импорт защищённой книги» и загрузите .json.
3. Выберите книгу и нажмите «Загрузить».
4. Введите те же Алфавит, Ключ и Раунды.
5. Нажмите «ВЫПОЛНИТЬ».

**КРИТИЧЕСКИ ВАЖНО:** Защищать можно только новую, ни разу не использованную книгу.

**ПОСЛЕ СОЗДАНИЯ ЗАЩИЩЁННОЙ КНИГИ:** удалите открытую книгу.

---

## 5. ШИФРОВАНИЕ ТЕКСТА

1. Выберите книгу и нажмите «Загрузить».
2. При загрузке введите алфавит, ключ и раунды Виженера.
3. При первой загрузке введите 4-символьный пароль продления таймера.
4. Введите сообщение.
5. Нажмите «ЗАШИФРОВАТЬ».
6. Результат — координаты вида страница:строка:символ.

**РЕКОМЕНДАЦИЯ ПО ДЛИНЕ:**
Для защиты от анализа трафика используйте фиксированную длину сообщений
(например, 40, 60 или 100 символов). Короткие сообщения добивайте мусором.

**ЗАЩИТА ОТ КЕЙЛОГЕРА:**
Включите «Виртуальная клавиатура». Раскладка случайна при каждом включении.

**СКРЫТИЕ ТЕКСТА:**
Текст скрыт точками. Удерживайте «👁 Удерживать для просмотра».

---

## 6. ДЕШИФРОВАНИЕ ТЕКСТА

1. Выберите книгу и нажмите «Загрузить».
2. Вставьте координаты в поле «Шифр».
3. Нажмите «РАСШИФРОВАТЬ».
4. Результат скрыт точками. Удерживайте «👁» для просмотра.

---

## 7. ТАЙМЕР БЕЗОПАСНОСТИ (Web Worker)

Единый таймер на 2 минуты на обе загруженные книги. Работает в фоновом
Web Worker и НЕ приостанавливается при блокировке экрана. Время — реальное.

Истечение таймера — окно продления 15 секунд. Нужно ввести пароль.
Неверный пароль или бездействие — все книги шифруются, поля очищаются.

---

## 8. СТАТИСТИКА И АУДИТ КНИГИ

1. Выберите книгу в блоке «Статистика».
2. Нажмите «Показать».
3. «Просмотреть книгу» — содержимое в отдельном окне. [_] = удалено.
4. «Резервная копия» — сохранить в .json.

---

## 9. ДВУХКНИЖНАЯ СХЕМА

— Вы шифруете своей книгой (Книга-1). Собеседник расшифровывает её копией.
— Собеседник шифрует своей книгой (Книга-2). Вы расшифровываете её копией.

---

## 10. НАСТРОЙКИ

— Язык: Русский / English.
— Тема: 🌙/☀️.
— Панель таймера: плавающая снизу.

---

## 11. КРИТИЧЕСКИЕ ПАРАМЕТРЫ БЕЗОПАСНОСТИ

### 11.1. ТРЕБОВАНИЯ К КНИГЕ
— «Крипто-генератор» включён (crypto.getRandomValues).
— ВСЕ наборы символов выбраны.
— Размер книги подбирается под нагрузку.

Расчёт ёмкости (10 000 ячеек):
При стандартной длине 40 символов — ~250 сообщений.
7 книг (по одной на день) — примерно на год общения.

### 11.2. ПРИНЦИП СВЕЖЕСТИ КНИГИ
Чем чаще заменяется книга — тем лучше. Как вода в баке.
Рекомендации: малая нагрузка — раз в месяц, средняя — раз в неделю,
высокая — каждый день или сеанс.

### 11.3. УТИЛИЗАЦИЯ ИСПОЛЬЗОВАННОЙ КНИГИ
Книга с дырами бесполезна. Перешли на новую — удалите старую.
Противник, захвативший использованную книгу, не получает НИЧЕГО.

### 11.4. ТРЕБОВАНИЯ К КЛЮЧУ ВИЖЕНЕРА
— Длина: не менее 40–60 символов.
— Ключ должен быть случайным.
— Хранить отдельно от книги.

### 11.5. ТРЕБОВАНИЯ К АЛФАВИТУ ВИЖЕНЕРА
— Максимальная длина.
— Нажать «Перемешать» для изменения порядка.
— Получатель использует ТОЧНО такой же.

---

## 12. ЗАЩИТА ОТ АНАЛИЗА ТРАФИКА

1. Договоритесь о СТАНДАРТНОЙ ДЛИНЕ (40, 60 или 100 символов).
2. Короткие сообщения добивайте мусором до стандартной длины.
3. При расшифровке читайте начало, игнорируйте хвост из мусора.
4. Счётчик символов помогает контролировать длину.

---

## 13. УСТОЙЧИВОСТЬ К АКТИВНОМУ ВМЕШАТЕЛЬСТВУ (MITM)

Если противник изменяет координату — получатель видит КАШУ вместо текста.
Это сразу обнаруживается. Противник НЕ МОЖЕТ подменить сообщение незаметно.
Содержание переписки остаётся защищённым.

---

## 14. РАСШИРЕННЫЕ СХЕМЫ УПРАВЛЕНИЯ КНИГАМИ

**14.1. БАЗОВАЯ РОТАЦИЯ: ОДНА КНИГА В ДЕНЬ**
7 комплектов книг. Каждый день — своя книга. Компрометация одного дня
раскрывает только этот день.

**14.2. ПРОДВИНУТАЯ РОТАЦИЯ: КОМПЛЕКТ НА КАЖДЫЙ СЕАНС**
N комплектов книг. Один сеанс — один комплект. После сеанса — удалить.

---

## 15. ПЕРЕДАЧА НОВЫХ КЛЮЧЕЙ ЧЕРЕЗ ИСТОЩЁННУЮ КНИГУ

Когда книга на исходе, новые параметры передаются в шифрованном сообщении.
Первая личная встреча нужна только ОДИН РАЗ. Далее система самоподдерживается.

---

## 16. РУЧНОЙ РЕЖИМ: ШИФРОВАНИЕ НА БУМАГЕ

### 16.1. КОГДА ЭТО НУЖНО
— Нет электричества или устройств.
— Полная изоляция от цифрового следа.

### 16.2. ПОДГОТОВКА КНИГИ ДЛЯ ПЕЧАТИ
Распечатайте книгу. Создайте ДВА экземпляра. Передайте получателю.

**СПОСОБЫ ПЕРЕДАЧИ:**
— Личная встреча (наиболее надёжный).
— Закладка (наиболее анонимный).
— Доверенный курьер (запасной).

**ВАЖНО:** Первая передача — самая безопасная. Слежки ещё нет.

### 16.3. ШИФРОВАНИЕ ВРУЧНУЮ
Для каждого символа: найдите свободную ячейку, запишите координаты,
зачеркните ячейку. Добавьте мусор до стандартной длины.

### 16.4. ДЕШИФРОВАНИЕ ВРУЧНУЮ
Для каждой координаты: найдите ячейку, прочитайте символ, зачеркните.

### 16.5. СИНХРОНИЗАЦИЯ
Отправитель ищет ПЕРВУЮ свободную ячейку по порядку.
Это гарантирует синхронность книг.

### 16.6. ГЕНЕРАЦИЯ КНИГИ БЕЗ КОМПЬЮТЕРА
Способ 1 — игральные кости.
Способ 2 — мешок с жетонами.
Способ 3 — таблица случайных чисел.

### 16.7. ОСОБЕННОСТИ РУЧНОГО РЕЖИМА
Виженер не используется. Цифровой след отсутствует полностью.
Криптостойкость равна цифровому режиму.

### 16.8. УНИЧТОЖЕНИЕ
Использованную книгу — сжечь до пепла. Пепел развеять.

### 16.9. ЯЗЫКОВАЯ УНИВЕРСАЛЬНОСТЬ КНИГИ
Русская «А» и английская «A» — один звук. Книга не привязана к языку.
Любой язык можно записать через доступные символы.

### 16.10. НЕВОЗМОЖНОСТЬ ДОКАЗАТЕЛЬСТВА
Даже перебрав все варианты, противник получит тысячи осмысленных сообщений.
Все равновероятны. Доказать, какое было настоящим — невозможно.
Информация уничтожена.

---

# USER MANUAL (ENGLISH)

## 1. GENERAL DESCRIPTION

StrikeCipherSVA is a text encryption tool based on the book cipher method
with marking of used cells and deletion of the symbol in the cell
(Amnesic Book Cipher — symbol amnesia).

Single HTML file. No installation. No internet. All data in IndexedDB.

**CORE PRINCIPLE:**
Book = 3D array (Pages × Rows × Characters). Each character replaced by
coordinates of a random free cell. Used cell is DELETED — symbol destroyed,
null remains. Without the book, coordinates = random numbers.

**KEY DIFFERENCE:** Used cells are marked, symbol destroyed. Book does NOT
remember. Impossible to prove what was in any cell. Even with full book
access post factum, message content cannot be recovered.

---

## 2. BOOK MANAGEMENT

**TARGETED DELETION:** Select a book and click "DELETE SELECTED BOOK".
**COMPLETE PURGE:** "DELETE ALL BOOKS" — confirm.

---

## 3. CREATING A BOOK

1. Enter name. 2. Set dimensions: Pages, Rows, Chars per row.
3. Select character sets. 4. Enable "Crypto-generator". 5. Click "CREATE BOOK".

**IMPORT/EXPORT:** Import/export via .json files.

---

## 4. BOOK PROTECTION (VIGENÈRE)

**ENCRYPT A BOOK:** Select mode, load book, fill Alphabet/Key/Rounds (17),
click "EXECUTE". Export via "Export encrypted book".

**DECRYPT A BOOK:** Import protected book, enter same Alphabet/Key/Rounds.

**CRUCIAL:** Only protect never-used books. Delete open book after.

---

## 5. TEXT ENCRYPTION

1. Select book, click "Load". 2. Enter Vigenère parameters.
3. Enter 4-char timer password. 4. Type message. 5. Click "ENCRYPT".

Fixed message length recommended. Virtual keyboard available. Text hidden.

---

## 6. TEXT DECRYPTION

1. Load book. 2. Paste coordinates. 3. Click "DECRYPT". 4. Hold "👁" to reveal.

---

## 7. SECURITY TIMER (Web Worker)

2-minute timer. Real clock. Extend with password. Expiry = encrypt + clear.

---

## 8. BOOK STATISTICS & AUDIT

Select book. "Show" for stats. "View Book" for contents. [_] = deleted.

---

## 9. DUAL-BOOK SCHEME

Book-1: you encrypt, contact decrypts. Book-2: contact encrypts, you decrypt.

---

## 10. SETTINGS

Language: Русский / English. Theme: 🌙/☀️. Timer bar at bottom.

---

## 11. CRITICAL SECURITY PARAMETERS

### 11.1. BOOK REQUIREMENTS
Crypto-generator on. ALL character sets. Book size per load.

### 11.2. BOOK FRESHNESS PRINCIPLE
Replace often. Light: monthly. Medium: weekly. Heavy: daily/session.

### 11.3. DISPOSAL OF USED BOOKS
Book with holes is useless. Delete immediately after switch.

### 11.4. VIGENÈRE KEY REQUIREMENTS
40–60 characters, random, stored separately.

### 11.5. VIGENÈRE ALPHABET REQUIREMENTS
Maximum length, shuffled. Recipient uses EXACT same.

---

## 12. TRAFFIC ANALYSIS PROTECTION

Standard message length. Short messages padded. Ignore tail on decrypt.

---

## 13. RESISTANCE TO ACTIVE INTERFERENCE (MITM)

Changed coordinate = GARBAGE. Immediately detected. Content stays protected.

---

## 14. ADVANCED BOOK MANAGEMENT SCHEMES

**14.1. DAILY ROTATION:** 7 sets, one per day.
**14.2. SESSION ROTATION:** N sets, one per session, destroy after.

---

## 15. TRANSMITTING NEW KEYS VIA DEPLETED BOOK

New parameters in encrypted message. One meeting only. System self-sustains.

---

## 16. MANUAL MODE: PAPER-ONLY ENCRYPTION

### 16.1. WHEN THIS IS NEEDED
No electricity. No devices. Complete isolation from digital traces.

### 16.2. PREPARING A BOOK FOR PRINTING
Print, two copies. Transfer methods: in-person, dead drop, courier.
First transfer is safest — no surveillance yet.

### 16.3. ENCRYPTING BY HAND
Find free cell, write coordinates, cross out. Add padding.

### 16.4. DECRYPTING BY HAND
Find cell by coordinates, read character, cross out.

### 16.5. SYNCHRONIZATION
Search FIRST free cell in order. Guarantees synchronization.

### 16.6. GENERATING A BOOK WITHOUT A COMPUTER
Dice, token bag, random number table.

### 16.7. CHARACTERISTICS
No Vigenère. No digital footprint. Equal strength to digital mode.

### 16.8. DESTRUCTION
Burn. To ashes. Scatter.

### 16.9. LANGUAGE UNIVERSALITY
Characters = sounds. Any language via transliteration. Book is universal.

### 16.10. IMPOSSIBILITY OF PROOF
Thousands of meaningful variants. All equally possible. Proof impossible.
Information destroyed. By any power. By any computer. Ever.

---

## Лицензия / License

MIT License © Cipher_MasterSVA, 2026.
