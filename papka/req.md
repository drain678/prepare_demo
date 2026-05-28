Windows (PowerShell)
Через pip:
```bash
py -m pip install --user uv
```
Проверка:
```bash
uv --version
```




pyproject.toml — создается командой uv init (или уже лежит в шаблоне проекта), потом дополняется зависимостями через uv add ....
uv.lock — генерируется командой uv lock (и автоматически обновляется при uv sync / uv add).
Типичный сценарий:

```bash
uv init
```
```bash
uv add django pillow psycopg2-binary
```
```bash
uv lock
```
потом обязательно эта команда: (и после изменений, типа добавляния новых зависимостей)
```bash
uv sync
```
Что это значит в вашем проекте:

pyproject.toml хранит декларацию проекта и зависимостей (что нужно).
uv.lock фиксирует точные версии (что именно установится у всех одинаково).