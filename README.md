# Todo App

Todo App складається з трьох Docker-сервісів:

- `frontend` — Angular SSR, доступний на порту `4200`;
- `backend` — ASP.NET Core Web API, доступний на порту `8080`;
- `sqlserver` — Microsoft SQL Server 2022, доступний на порту `1433`.

## Клонування репозиторію

`backend` і `frontend` підключені до головного репозиторію як Git-сабмодулі. Щоб одразу завантажити головний проєкт разом із ними, виконайте:

```powershell
git clone --recurse-submodules git@github.com:olehskomorokha/todo-app.git
cd todo-app
```

Для SSH-клонування на комп'ютері має бути налаштований SSH-ключ із доступом до GitHub.

Якщо головний репозиторій уже клонований без сабмодулів, перейдіть у його каталог і виконайте:

```powershell
git submodule update --init --recursive
```

Після отримання нових змін головного репозиторію підтягнути зафіксовані в ньому версії сабмодулів можна так:

```powershell
git pull
git submodule update --init --recursive
```

Перевірити стан сабмодулів:

```powershell
git submodule status
```

Символ `-` перед хешем означає, що сабмодуль ще не ініціалізований. Символ `+` означає, що локально вибрано інший коміт, ніж зафіксований у головному репозиторії.

## Налаштування

У корені проєкту створіть файл `.env`:

```env
MSSQL_SA_PASSWORD=Your_Strong_Password1!
JWT_KEY=Your_JWT_Secret_Key_At_Least_32_Characters_Long
JWT_ISSUER=ToDoApp
JWT_AUDIENCE=ToDoAppUsers
```

Пароль має містити щонайменше 8 символів і символи принаймні трьох категорій: великі літери, малі літери, цифри та спеціальні символи.

`JWT_KEY` — секретний ключ для підпису JWT-токенів. Передайте його через `.env`; значення повинно містити щонайменше 32 символи. Окремо генерувати ключ PowerShell-командами не обов'язково — можна вказати власний довгий непередбачуваний рядок.

Файл `.env` виключений із Git. Не публікуйте та не передавайте його разом із вихідним кодом. Під час розгортання на іншому комп'ютері потрібно створити новий `.env` із новими секретами.

Структура файлів для Docker-збірки:

```text
todo-app/
├── docker-compose.yml
├── .env
├── backend/
│   ├── Dockerfile
│   ├── TodoApp.Controllers/
│   ├── TodoApp.Data/
│   ├── TodoApp.Interfaces/
│   └── TodoApp.Services/
└── frontend/
    └── todo-app/
        ├── Dockerfile
        ├── package.json
        └── src/
```

## Збірка і запуск

Відкрийте PowerShell у корені проєкту та виконайте:

```powershell
docker compose up --build
```

Під час першого запуску Docker завантажить базові образи, збере backend і frontend та запустить SQL Server. Це може зайняти кілька хвилин.

Щоб запустити контейнери у фоновому режимі:

```powershell
docker compose up --build -d
```

Перевірити стан контейнерів:

```powershell
docker compose ps
```

Перевірити логи всіх сервісів:

```powershell
docker compose logs -f
```

Docker Compose передає `JWT_KEY`, `JWT_ISSUER` та `JWT_AUDIENCE` з `.env` у backend-контейнер. Якщо `JWT_KEY` не заданий, Compose зупинить запуск із повідомленням `Set JWT_KEY in .env`. Environment-змінні контейнера можна переглянути через `docker inspect`, тому для production використовуйте захищене сховище секретів.

## Міграції бази даних

EF Core міграції застосовуються backend-сервісом автоматично під час запуску через `Database.MigrateAsync()`.

Перебіг міграцій і можливі помилки підключення можна перевірити в логах:

```powershell
docker compose logs -f backend
```

## Адреси сервісів

- frontend: <http://localhost:4200>
- backend API: <http://localhost:8080>
- Swagger UI: <http://localhost:8080/swagger>
- SQL Server: `localhost,1433`


## Зупинка

Зупинити та видалити контейнери, зберігши дані SQL Server:

```powershell
docker compose down
```

Зупинити проєкт і видалити також базу даних:

```powershell
docker compose down --volumes
```
