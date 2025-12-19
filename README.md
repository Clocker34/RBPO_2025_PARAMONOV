# Доска объявлений (Spring Boot)

Учебный REST‑сервис «Доска объявлений», реализованный на Spring Boot.  
Проект демонстрирует работу с базой данных через Spring Data JPA и набор бизнес‑операций поверх базового CRUD для сущностей доски объявлений [web:460][web:466].

## Технологии

- Java 17+
- Spring Boot 3.5.5+
- Spring Web
- Spring Data JPA
- PostgreSQL
- Maven [web:461][web:468]

## Быстрый старт

### 1. Клонирование репозитория

git clone https://github.com/MatorinFedor/RBPO_2025_demo.git
cd RBPO_2025_demo

text

### 2. Настройка базы данных

В `application.properties` / `application.yml` должны быть указаны настройки подключения к PostgreSQL  
(имя БД, пользователь, пароль). При первом запуске Hibernate создаст таблицы `users`, `categories`, `listings`, `messages`, `reports` [web:257].

### 3. Запуск приложения

Запуск через IDE (класс `BulletinBoardApplication`) или из командной строки:

mvn spring-boot:run

text

По умолчанию сервис доступен по адресу `http://localhost:8080`.

## Структура проекта

src/main/java/ru/mtuci/bulletin_board/
├── controller/
│ ├── ListingController.java
│ ├── CategoryController.java
│ ├── UserController.java
│ ├── MessageController.java
│ └── ReportController.java
├── entity/
│ ├── Listing.java
│ ├── Category.java
│ ├── User.java
│ ├── Message.java
│ ├── Report.java
│ └── ReportStatus.java
├── repository/
│ ├── ListingRepository.java
│ ├── CategoryRepository.java
│ ├── UserRepository.java
│ ├── MessageRepository.java
│ └── ReportRepository.java
└── BulletinBoardApplication.java

text

## Сущности доменной области

- **Listing** — объявление с заголовком, описанием и связями с автором (`User`) и категорией (`Category`).  
- **Category** — категория объявлений (например, «Техника», «Услуги», «Недвижимость», «Велосипеды»).  
- **User** — пользователь сервиса, автор объявлений, сообщений и жалоб.  
- **Message** — сообщение к объявлению для обсуждения условий.  
- **Report** — жалоба на объявление, связана с объявлением, пользователем и имеет статус `NEW / IN_PROGRESS / CLOSED` (enum `ReportStatus`) [web:462][web:463].

## CRUD‑эндпоинты

Все CRUD‑операции работают с реальной базой данных через Spring Data JPA.

### Объявления (Listing)

- `GET /api/listings` — получить все объявления  
- `GET /api/listings/{id}` — получить объявление по id  
- `POST /api/listings` — создать объявление (в теле JSON с вложенными `author.id` и `category.id`)  
- `PUT /api/listings/{id}` — обновить объявление  
- `DELETE /api/listings/{id}` — удалить объявление

### Категории (Category)

- `GET /api/categories` — получить все категории  
- `GET /api/categories/{id}` — получить категорию по id  
- `POST /api/categories` — создать категорию  
- `PUT /api/categories/{id}` — обновить категорию  
- `DELETE /api/categories/{id}` — удалить категорию

### Пользователи (User)

- `GET /api/users` — получить всех пользователей  
- `GET /api/users/{id}` — получить пользователя по id  
- `POST /api/users` — создать пользователя  
- `PUT /api/users/{id}` — обновить пользователя  
- `DELETE /api/users/{id}` — удалить пользователя

### Сообщения (Message)

- `GET /api/messages` — получить все сообщения  
- `GET /api/messages/{id}` — получить сообщение по id  
- `POST /api/messages` — создать сообщение  
- `PUT /api/messages/{id}` — обновить сообщение  
- `DELETE /api/messages/{id}` — удалить сообщение

### Жалобы (Report)

- `GET /api/reports` — получить все жалобы  
- `GET /api/reports/{id}` — получить жалобу по id  
- `POST /api/reports` — создать жалобу на объявление  
- `PUT /api/reports/{id}` — обновить жалобу (при необходимости)  
- `DELETE /api/reports/{id}` — удалить жалобу [web:414][web:419]

## Бизнес‑операции поверх CRUD

Реализован набор операций, отражающих бизнес‑логику доски объявлений:

1. **Поиск объявлений по категории и автору**

   - `GET /api/listings/search?categoryId={categoryId}&authorId={authorId}`  
   - Возвращает объявления, у которых совпадают и категория, и автор.

2. **Поиск по тексту и категории**

   - `GET /api/listings/search-by-text?text={text}&categoryId={categoryId}`  
   - Ищет по подстроке в заголовке (`title`) без учёта регистра в указанной категории.

3. **Все объявления пользователя**

   - `GET /api/listings/by-author/{authorId}`  
   - Возвращает все объявления выбранного пользователя.

4. **Подача жалобы на объявление**

   - `POST /api/reports?listingId={listingId}&authorId={authorId}&reason={reason}`  
   - Создаёт запись `Report` со статусом `NEW` и связями с объявлением и пользователем.

5. **Изменение статуса жалобы**

   - `PUT /api/reports/{id}/status?value=NEW|IN_PROGRESS|CLOSED`  
   - Обновляет поле `status` у жалобы (enum `ReportStatus`) [web:371][web:381][web:409].

## Тестирование через Postman

В репозитории предусмотрена коллекция Postman (или можно собрать вручную) для проверки типового сценария:

1. Создать пользователя (`POST /api/users`).  
2. Создать одну или несколько категорий (`POST /api/categories`).  
3. Создать объявления с привязкой к пользователю и категориям (`POST /api/listings`).  
4. Выполнить бизнес‑поиски:
   - по автору и категории;
   - по тексту и категории;
   - все объявления пользователя.  
5. Создать жалобу на объявление (`POST /api/reports`).  
6. Изменить статус жалобы (`PUT /api/reports/{id}/status`) и проверить результат через `GET /api/reports/{id}` [web:465][web:467].

## Авторство

Проект создан в рамках учебной практики по Spring Boot в МТУСИ.
