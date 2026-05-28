как создать venv на винде ебаной

создать:
```bash
py -3.13 -m venv .venv
```
активировать:
```bash
.\.venv\Scripts\Activate.ps1
```
Если PowerShell ругается на policy:
```bash
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```
проверка:
```bash
python --version
pip --version
```