# Todo App

Todo App складається з трьох Docker-сервісів:

- `frontend` — Angular SSR, доступний на порту `4200`;
- `backend` — ASP.NET Core Web API, доступний на порту `8080`;
- `sqlserver` — Microsoft SQL Server 2022, доступний на порту `1433`.

## Налаштування

У корені проєкту створіть файл `.env`:

```env
MSSQL_SA_PASSWORD=Your_Strong_Password1!
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