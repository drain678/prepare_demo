1.
django-admin startproject config .
python manage.py startapp core

2. Подключить приложение и БД в настройках
В config/settings.py:

3. добавить core в INSTALLED_APPS
настроить DATABASES (PostgreSQL: NAME, USER, PASSWORD, HOST, PORT)

4.
python manage.py makemigrations
python manage.py migrate

5.
python manage.py runserver