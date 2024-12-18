# Проект FastAPI: Планировщик дел TODO и Сервис Сокращения URL
Этот репозиторий содержит два микросервиса, разработанных с использованием FastAPI: планировщик дел TODO и сервис сокращения URL. Оба сервиса упакованы в Docker-контейнеры и используют базы данных SQLite для хранения данных.
## Требования
Для локального запуска этих сервисов понадобятся:

- Python 3.7+
- SQLite
- Docker (для запуска в контейнерах)
- Docker Compose (рекомендуется для упрощённого управления контейнерами)
## Установка
1. Клонируйте этот репозиторий:
```
git clone https://github.com/katrinnaya/my-fastapi-project.git
```
2. Перейдите в директорию проекта:
```
cd my-fastapi-project
```
## Локальный запуск (без Docker)
1. Установите зависимости для каждого сервиса:
### Для приложения TODO
``` 
pip install -r todo_app/requirements.txt
```
### Для сервиса сокращения URL
```
pip install -r shorturl_app/requirements.txt
```
2. Запустите сервисы:
### Для приложения TODO
```
cd todo_app
uvicorn main:app --reload
```
### Для сервиса сокращения URL
```
cd ../shorturl_app
uvicorn main:app --reload
```
### Теперь сервисы будут доступны по следующим адресам:
- Приложение TODO: http://localhost:8000/docs
- Сервис сокращения URL: http://localhost:8001/docs

## Запуск через Docker
1. Постройте Docker-образы для обоих сервисов:
### Для приложения TODO
```
docker build -t todo-app todo_app/
```
### Для сервиса сокращения URL
```
docker build -t shorturl-service shorturl_app/
```
2. Создайте именованные тома для каждого сервиса:
```
docker volume create todo_data
docker volume create shorturl_data
```
3. Запустите контейнеры:
### Для приложения TODO
```
docker run -d -p 8000:80 -v todo_data:/app/data todo-app
```
### Для сервиса сокращения URL
```
docker run -d -p 8001:80 -v shorturl_data:/app/data shorturl-service
```
## Теперь сервисы будут доступны по тем же адресам:
- Приложение TODO: http://localhost:8000/docs
- Сервис сокращения URL: http://localhost:8001/docs

## Загрузка готового образа из Docker Hub
### Для приложения TODO
```
docker pull katrinnaya/todo-app
```
### Для сервиса сокращения URL
```
docker pull katrinnaya/shorturl_app
```
## Тестирование эндпоинтов
Вы можете протестировать работу эндпоинтов с помощью curl или любого другого инструмента для тестирования API, такого как Postman.
### Приложение TODO
- Создание новой задачи (POST /items)
```
curl -X POST \
  -H 'Content-Type: application/json' \
  -d '{"title": "Новая задача", "description": "Описание задачи"}' \
  http://localhost:8000/items
```
- Получение всех задач (GET /items) 
```
curl http://localhost:8000/items
```
- Получение конкретной задачи (GET /items/{item_id})
``` 
curl http://localhost:8000/items/1
```
- Обновление задачи (PUT /items/{item_id})
```
curl -X PUT \
  -H 'Content-Type: application/json' \
  -d '{"title": "Обновленная задача", "completed": true}' \
  http://localhost:8000/items/1
```
- Удаление задачи (DELETE /items/{item_id})
```
curl -X DELETE http://localhost:8000/items/1
```
- Получение количества завершённых задач (GET /completed-items-count)
```
curl http://localhost:8000/completed-items-count
```
### Сервис сокращения URL
- Создание короткой ссылки (POST /shorten)
```
curl -X POST \
  -H 'Content-Type: application/json' \
  -d '{"original_url": "https://example.com/dlinnyj-url"}' \
  http://localhost:8001/shorturls
```
- Получение всех сокращённых URL (GET /all-urls)
```
curl -X GET http://localhost:8000/all-urls
```
- Переадресация на оригинальный URL (GET /{short_id})
```
curl http://localhost:8001/{short_id}
```
- Просмотр данных по сокращенной ссылке (GET /stats/{short_id})
```
curl http://localhost:8001/stats/{short_id}
```
## Лицензия
Этот проект распространяется под лицензией MIT. Подробности смотрите в файле ```LICENSE```.
