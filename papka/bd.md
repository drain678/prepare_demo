Можно подключиться к бд через дибивер, там уже есть соединение и нужно просто создать бд с нужным названием.

cat hui/core/models.py
mv hui/core/models.py core/models.py
copy hui/core/models.py core/models.py


pg_dump -U postgres -h localhost -p 54320 -Fp postgres -f idk.sql


1. Запуск сервиса PostgreSQL на винде
Обычно сервис запускается сам. Проверить/запустить в PowerShell:
``` bash
Get-Service *postgres*
Start-Service postgresql-x64-17
```
Если имя сервиса немного другое, смотри в выводе Get-Service *postgres*.

2. Создать базу shoe_store_2
Открой SQL Shell (psql) из меню Пуск
или PowerShell и запусти (пример пути с обратными слешами):
``` bash
& "C:\Program Files\PostgreSQL\17\bin\psql.exe" -U postgres -h localhost -p 5432
```
Внутри psql:
``` bash
CREATE DATABASE shoe_store_2;
\l
\q
```


Команда Get-Service *postgres*
Что делает: ищет в Windows все службы, в имени которых есть postgres.
Зачем: после установки PostgreSQL сервер обычно работает как служба Windows. Имя может быть разным, например:

postgresql-x64-17
postgresql-x64-16
PostgreSQL 17

Без этой команды ты не узнаешь точное имя для Start-Service.
Пример вывода:
Status   Name               DisplayName
------   ----               -----------
Running  postgresql-x64-17  postgresql-x64-17



Команда Start-Service postgresql-x64-17
Что делает: запускает службу PostgreSQL (если она остановлена).
Зачем: Django и DBeaver подключаются к работающему серверу. Если служба не запущена — будет Connection refused.

Важно: postgresql-x64-17 — пример. Подставь имя из Get-Service *postgres*.

Остановить (если нужно перезапустить)
```bash
Stop-Service postgresql-x64-17
```
Если «Access denied»: открой PowerShell от имени администратора (правый клик → «Запуск от имени администратора»)



Команда: подключение к PostgreSQL через psql
& "C:\Program Files\PostgreSQL\17\bin\psql.exe" -U postgres -h localhost -p 5432
Зачем: войти в интерактивную консоль psql, чтобы выполнить CREATE DATABASE.
После команды обычно спросят пароль — тот, что задавала при установке PostgreSQL.
Альтернативы:
# Если psql уже в PATH (после установки)
psql -U postgres -h localhost -p 5432

Если путь другой: версия может быть 16, 15 — смотри папку: C:\Program Files\PostgreSQL\
Если psql не найден: добавь в PATH или всегда используй полный путь к bin.



Команды внутри psql:
После успешного входа приглашение будет вида: postgres=#

CREATE DATABASE shoe_store_2;
Что делает: создаёт новую базу данных с именем shoe_store_2.
Зачем: в settings.py у тебя "NAME": "shoe_store_2". Django подключается к конкретной базе, а не «ко всему PostgreSQL». Без этой команды была бы ошибка: database "shoe_store_2" does not exist.
Точка с запятой ; в SQL — конец команды.



Команда \l
Что делает: список всех баз на этом сервере (команда psql, не SQL).
Зачем: проверить, что shoe_store_2 появилась в списке.
Альтернатива (SQL):
SELECT datname FROM pg_database;


\q
Что делает: выход из psql обратно в PowerShell.
Зачем: закончить сессию; дальше работаешь с Django (migrate, runserver).
Альтернатива: Ctrl+D или закрыть окно.





1) Как добавить PostgreSQL в PATH на Windows
Нужно добавить папку с psql.exe, обычно:

C:\Program Files\PostgreSQL\17\bin

(если версия 16 — замени 17 на 16).

Способ A — через интерфейс (проще запомнить)
Win → в поиске: «переменные среды» → «Изменение системных переменных среды».
Кнопка «Переменные среды…».
В блоке «Переменные среды пользователя» (или «системные», если нужно для всех) выбери Path → «Изменить».
«Создать» → вставь:
C:\Program Files\PostgreSQL\17\bin
ОК везде.
Закрой и заново открой PowerShell / VS Code Terminal.
Проверка:
```bash
psql --version
```
Должна показаться версия PostgreSQL.

Способ B — через PowerShell (только для текущего пользователя)
```bash
$pgBin = "C:\Program Files\PostgreSQL\17\bin"
[Environment]::SetEnvironmentVariable(
    "Path",
  [Environment]::GetEnvironmentVariable("Path", "User") + ";$pgBin",
    "User"
)
```
Потом перезапусти терминал и проверь:
```bash
psql --version
```
Если PATH не сработал
Всегда можно вызывать по полному пути:
```bash
& "C:\Program Files\PostgreSQL\17\bin\psql.exe" -U postgres -h localhost
```



Команда: создать роль postgres на Windows
Сначала зайди в psql под существующим суперпользователем (часто это тот, кого создали при установке — часто тоже postgres, иногда другой).

Вариант 1 — через createuser (удобнее)
```bash
& "C:\Program Files\PostgreSQL\17\bin\createuser.exe" -U postgres -h localhost -p 5432 -s postgres
```
-s — суперпользователь (права на всё, как у админа БД).
Если роль уже есть, будет ошибка already exists — это нормально.

Вариант 2 — через SQL в psql
```bash
& "C:\Program Files\PostgreSQL\17\bin\psql.exe" -U postgres -h localhost -d postgres
```
Внутри:
```bash
CREATE ROLE postgres WITH LOGIN SUPERUSER PASSWORD '826456';
```
Пароль '826456' — как в твоём settings.py; на экзамене лучше свой, но тот же в DBeaver и в Django.

Проверить роли:
\du


