# Effective-Mobile project

Проект создан для прохождения тестового задания в компанию **Effective Mobile** на позицию **DevOps Junior Engineer**.

## Описание проекта

Проект представляет собой простой веб-сервис, который принимает HTTP-запросы и возвращает текстовый ответ пользователю.

Сервис запускается через **Docker Compose**.

При обращении к главной странице сервис выводит сообщение:

```text
Hello from Effective Mobile!
```

## Используемый стек

| Компонент      | Описание                                                            |
| -------------- | ------------------------------------------------------------------- |
| Backend        | Написан на Python версии 3.13                                       |
| Python library | Используется стандартная библиотека `http.server`                   |
| Nginx          | Версия 1.30.2, используется как reverse proxy                       |
| Docker         | Контейнеризация backend и nginx                                     |
| Docker Compose | Совместный запуск контейнеров и настройка взаимодействия между ними |

## Backend

Backend доступен только внутри Docker-сети на порту, указанном в `.env` файле.

В файле `main.py` используется порт:

```text
8080
```

Если нужно использовать другой порт, его необходимо изменить:

1. в файле `main.py`;
2. в файле `.env`.

## Nginx

Nginx доступен внешне на порту, указанном в `.env` файле.

Внутри Docker-сети используется порт, который также указан в `.env`.

По умолчанию Nginx слушает `80` порт и проксирует запросы на backend:

```text
http://backend:8080
```

Для передачи информации о клиентском запросе используются директивы:

```nginx
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

## Структура проекта

```text
Effective-Mobile/
├── backend/
│   ├── Dockerfile
│   └── main.py
├── nginx/
│   ├── Dockerfile
│   └── nginx.conf
├── .env.example
├── docker-compose.yml
└── README.md
```

## Как скачать репозиторий

Склонировать репозиторий:

```bash
git clone https://github.com/tut-svaga/Effective-Mobile
```

Перейти в папку проекта:

```bash
cd Effective-Mobile
```

## Как настроить `.env`

Скопировать файл `.env.example` и создать на его основе `.env`:

```bash
cp .env.example .env
```

После этого можно настроить значения портов в `.env` файле.

## Как запустить проект

Запуск с пересборкой контейнеров:

```bash
docker compose up --build
```

Запуск в фоне:

```bash
docker compose up --build -d
```

## Проверка работы

Выполнить запрос:

```bash
curl http://localhost
```

Ожидаемый ответ:

```text
Hello from Effective Mobile!
```

## Назначение Docker Compose

Docker Compose используется для совместного запуска контейнеров backend и nginx.

Также он нужен для:

* настройки общей Docker-сети;
* передачи переменных окружения;
* определения порядка запуска сервисов;
* удобного запуска всего проекта одной командой.

## Назначение Nginx

Nginx используется как reverse proxy.

Он принимает внешние HTTP-запросы и перенаправляет их во внутренний backend-сервис.

Это позволяет не открывать backend напрямую наружу, а обращаться к нему через Nginx.

## Tut-svaga

Тестовое задание выполнено для компании **Effective Mobile**.

Репозиторий:

```text
https://github.com/tut-svaga/Effective-Mobile
```
