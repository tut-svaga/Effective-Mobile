# Effective Mobile

Простой HTTP-сервис, запущенный через Docker Compose.

При обращении к главной странице сервис возвращает:

```text
Hello from Effective Mobile!
```

## Стек

- Python 3.13
- Nginx 1.30.2
- Docker
- Docker Compose

## Как работает

Запрос от пользователя сначала попадает в Nginx.

Nginx проксирует запрос во внутренний backend-сервис:

```text
Client -> Nginx -> Backend
```

Backend доступен только внутри Docker-сети.  
Наружу открыт только Nginx.

## Структура проекта

```text
.
├── backend/
│   ├── Dockerfile
│   ├── main.py
│   └── .dockerignore
├── nginx/
│   ├── Dockerfile
│   ├── nginx.conf
│   └── .dockerignore
├── docker-compose.yaml
├── .env.example
├── .gitignore
└── README.md
```

## Архитектура

```
Client -> Nginx -> Backend
```

## Переменные окружения

Для настройки портов используется файл `.env`.

В репозитории хранится только пример файла:

```env
BACK_EXPOSE=8080
NGINX_HOST_PORT=80
NGINX_CONT_PORT=80
```

Файл `.env` не добавляется в репозиторий, так как может содержать локальные настройки или секретные данные.

## Запуск

Скопировать пример переменных окружения:

```bash
cp .env.example .env
```

Запустить проект:

```bash
docker compose up --build
```

Проверить работу:

```bash
curl http://localhost
```

Ожидаемый ответ:

```text
Hello from Effective Mobile!
```

## Остановка

```bash
docker compose down
```

## CI

В проект добавлена базовая проверка через GitHub Actions.

При push в main и dev workflow собирает проект через Docker Compose, запускает контейнеры и проверяет HTTP-ответ от Nginx.

## Что сделано

- backend-сервис на Python;
- Nginx настроен как reverse proxy;
- сервисы запускаются через Docker Compose;
- backend не открывается наружу напрямую;
- настройки портов вынесены в `.env`;
- `.env` исключён из репозитория.