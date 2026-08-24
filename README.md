# Todo App

Todo App складається з трьох Docker-сервісів:

- `frontend` — Angular SSR, доступний на порту `4200`;
- `backend` — ASP.NET Core Web API, доступний на порту `8080`;
- `sqlserver` — Microsoft SQL Server 2022, доступний на порту `1433`.

## Клонування репозиторію

`backend` і `frontend` підключені до головного репозиторію як Git-сабмодулі. Щоб одразу завантажити головний проєкт разом із ними, виконайте:

```powershell
git clone --recurse-submodules git@github.com:olehskomorokha/todo-app.git
```

## Налаштування

У корені проєкту створіть файл `.env`:

```env
MSSQL_SA_PASSWORD=Your_Strong_Password1!
JWT_KEY=Your_JWT_Secret_Key_At_Least_32_Characters_Long
JWT_ISSUER=ToDoApp
JWT_AUDIENCE=ToDoAppUsers
```

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

Відкрийте термінал у корені проєкту та виконайте:

```
docker compose up -d --build
```

## Адреси сервісів

- frontend: <http://localhost:4200>
- backend API: <http://localhost:8080>
- Swagger UI: <http://localhost:8080/swagger>
- SQL Server: `localhost,1433`


## Зупинка

Зупинити та видалити контейнери, зберігши дані SQL Server:

```
docker compose down -v
```

