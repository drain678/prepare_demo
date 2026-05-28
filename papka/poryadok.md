Создать папку проекта и открыть в VS Code.

Создать venv и активировать.

Установить зависимости (django, psycopg2-binary, pillow, при желании uv).

Создать Django-проект (django-admin startproject ...) и приложение (python manage.py startapp core). И создать папки static/css; static/images; core/templates/core; media/

Описать модели (схему БД в Django).

Настроить PostgreSQL в settings.py.

Запустить PostgreSQL и создать БД.

makemigrations + migrate (создать таблицы в БД).

Подготовить CSV из Excel.

Импортировать данные (кастомная команда import_data.py).

Проверить в DBeaver и запустить runserver.