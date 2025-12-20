


# Доска объявлений (Spring Boot)

Учебный REST‑сервис **«Доска объявлений»**, реализованный на Spring Boot.  
Проект демонстрирует работу с базой данных через Spring Data JPA и бизнес‑операции поверх базового CRUD для сущностей доски объявлений.

## Технологии

- Java 17+
- Spring Boot 3.5.5+
- Spring Web
- Spring Data JPA
- PostgreSQL
- Maven

## Быстрый старт

### 1. Клонирование репозитория

git clone https://github.com/Clocker34/RBPO_2025_PARAMONOV.git
cd RBPO_2025_PARAMONOV

text

### 2. Настройка базы данных

В `application.properties` / `application.yml` укажи настройки подключения к PostgreSQL (имя БД, пользователь, пароль).  
При первом запуске Hibernate создаст таблицы `users`, `categories`, `listings`, `messages`, `reports`.

### 3. Запуск приложения

Запуск через IDE (класс `BulletinBoardApplication`) или:

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
- **Category** — категория объявлений (например, «Техника», «Услуги», «Недвижимость»).
- **User** — пользователь сервиса, автор объявлений, сообщений и жалоб.
- **Message** — сообщение к объявлению для обсуждения условий.
- **Report** — жалоба на объявление со статусом `NEW` / `IN_PROGRESS` / `CLOSED` (enum `ReportStatus`).

## CRUD‑эндпоинты

### Объявления (Listing)

- `GET /api/listings`
- `GET /api/listings/{id}`
- `POST /api/listings`
- `PUT /api/listings/{id}`
- `DELETE /api/listings/{id}`

### Категории (Category)

- `GET /api/categories`
- `GET /api/categories/{id}`
- `POST /api/categories`
- `PUT /api/categories/{id}`
- `DELETE /api/categories/{id}`

### Пользователи (User)

- `GET /api/users`
- `GET /api/users/{id}`
- `POST /api/users`
- `PUT /api/users/{id}`
- `DELETE /api/users/{id}`

### Сообщения (Message)

- `GET /api/messages`
- `GET /api/messages/{id}`
- `POST /api/messages`
- `PUT /api/messages/{id}`
- `DELETE /api/messages/{id}`

### Жалобы (Report)

- `GET /api/reports`
- `GET /api/reports/{id}`
- `POST /api/reports`
- `PUT /api/reports/{id}`
- `DELETE /api/reports/{id}`

## Бизнес‑операции поверх CRUD

- **Поиск объявлений по категории и автору**  
  `GET /api/listings/search?categoryId={categoryId}&authorId={authorId}`

- **Поиск по тексту и категории**  
  `GET /api/listings/search-by-text?text={text}&categoryId={categoryId}`

- **Все объявления пользователя**  
  `GET /api/listings/by-author/{authorId}`

- **Подача жалобы на объявление**  
  `POST /api/reports?listingId={listingId}&authorId={authorId}&reason={reason}`

- **Изменение статуса жалобы**  
  `PUT /api/reports/{id}/status?value=NEW|IN_PROGRESS|CLOSED`

## Тестирование через Postman

Пример сценария:

1. Создать пользователя (`POST /api/users`).
2. Создать категории (`POST /api/categories`).
3. Создать объявления (`POST /api/listings`).
4. Выполнить поисковые запросы (по автору, категории, тексту).
5. Создать жалобу (`POST /api/reports`).
6. Обновить статус жалобы (`PUT /api/reports/{id}/status`) и проверить через `GET /api/reports/{id}`.

## Авторство

Проект создан в рамках учебной практики по Spring Boot в МТУСИ.
