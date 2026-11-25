# Timsco Connector v2.1.0 — Autodesk Revit 2022–2027 MCP App

**Разработчик / Developer:** **Timsco Partners LLC**, Bishkek, Kyrgyzstan  
**Релиз / Release:** [`v2.1.0`](https://github.com/sayedibraimov/timsco-connector/releases/tag/v2.1.0) · **Лицензия / License:** Proprietary / Timsco Partners LLC

**Timsco Connector** — локальное MCP-приложение (**MCP App**), библиотека из 11 горяче-загружаемых C#-модулей (`scripts/lib`) и MCP-сервер для двусторонней работы с **Autodesk Revit 2022–2027** в среде **Google Antigravity**.

Начиная с версии **v2.1.0**, интерфейс по умолчанию полностью переведён на **русский язык** с возможностью выбора **Русский / English** при установке и переключением `RU | EN` в реальном времени прямо в интерфейсе `ui/app.html`.

---

## Быстрая загрузка (Windows)

- **Установщик без прав администратора:** [`TimscoConnectorSetup_v2.1.0.exe`](https://github.com/sayedibraimov/timsco-connector/releases/download/v2.1.0/TimscoConnectorSetup_v2.1.0.exe) (`2242560 bytes` / `2190 KB`)
- **SHA-256:** `cdb4873ae6b9da54ef1ee25e32c295ca32884290fb8724a4cefa2c4edf2285ce`
- **Архив релиза:** [`TimscoConnector_v2.1.0_Release.zip`](https://github.com/sayedibraimov/timsco-connector/releases/download/v2.1.0/TimscoConnector_v2.1.0_Release.zip)

---

## Поддерживаемые версии Autodesk Revit

| Версии Revit | Целевой TFM / API | Изоляция и особенности |
|---|---|---|
| **2022 – 2023** | `net48` (`REVIT_LEGACY_ID`) | 32-битный `ElementId.IntegerValue` через `RevitCompat`, изолированный `System.Text.Json.dll` в `TimscoConnector\` |
| **2024** | `net48` (`long` `ElementId.Value`) | 64-битный ID на `.NET Framework 4.8`, изолированный `System.Text.Json.dll` в `TimscoConnector\` |
| **2025 – 2026** | `net8.0-windows` | Нативный `AssemblyLoadContext` с горячей выгрузкой и системный `System.Text.Json` |
| **2027** | `net10.0-windows` (`REVIT2027`) | Поддержка `CurveIntersectResultOption.Detailed` и API Revit 2027 |

---

## Локализация RU / EN (новое в v2.1.0)

1. **При установке (`TimscoConnectorSetup_v2.1.0.exe`):**
   - В шапке окна установки доступны кнопки **`RU`** (по умолчанию) и **`EN`** — весь интерфейс установщика переключается мгновенно.
   - Для тихой установки в нужном языке используйте флаг `/lang ru` или `/lang en`.
   - Установщик записывает выбранный язык в `%USERPROFILE%\.gemini\config\plugins\timsco-connector\config.json` (`{"language":"ru"}`) и устанавливает соответствующую версию `plugin.json`.
2. **В интерфейсе `ui/app.html`:**
   - В шапке панели справа расположен переключатель **`RU | EN`** — меняет все заголовки, вкладки, кнопки инструментов, описания типовых задач, статусы и подсказки без перезагрузки Revit (выбор сохраняется в `localStorage["timsco_ui_lang"]`).
3. **Для MCP-сервера (`RevitMcpServer.exe`):**
   - Приоритет языка: переменная окружения `TIMSCO_LANG=ru|en` → `config.json` → по умолчанию `ru`.

---

## Как работать с плагином и `app.html` в Google Antigravity

Подробная пошаговая инструкция вынесена в [ANTIGRAVITY_GUIDE.md](docs/ANTIGRAVITY_GUIDE.md). Краткий порядок работы:

1. **Откройте проект в Autodesk Revit (2022–2027).** После установки `TimscoConnectorSetup_v2.1.0.exe` надстройка автоматически поднимет версионный канал Named Pipe `\\.\pipe\rvt-mcp-pipe-<year>`.
2. **Откройте панель `app.html` в Google Antigravity:**
   - Выполните в чате Antigravity слэш-команду или запрос: `Открой Timsco Connector` / вызовите MCP-инструмент `open_revit_inspector` — интерфейс `ui/app.html` откроется прямо в чате или в боковой панели (**Auxiliary Pane** / `aux_pane`) с ресурсом `ui://revit/app.html`.
3. **Работайте без лишнего расхода токенов (`0 токенов`):**
   - **Показатели (Total Numbers):** сводные счётчики элементов, категорий, уровней, типоразмеров и предупреждений модели.
   - **Навигатор и параметры (Navigator & Parameters):** фильтрация элементов по категориям, просмотр/выбор элементов и пакетное изменение параметров в одной транзакции (`Ctrl+Z`).
   - **Семейства (Families):** просмотр загруженных семейств и тихая загрузка `.rfa` из `C:\ProgramData\Autodesk\RVT <year>\Libraries\...` (год Revit определяется автоматически).
   - **Скрипты и быстрые задачи (Quick Scripts & Prompts):**
     - Левая колонка — прямой запуск `22` готовых DLL-команд из `scripts/lib/out/<year>/*.dll` (`DatumTools`, `ArchModelTools`, `AuditTools`, `ParamTools`, `JoinTools`, `CleanupTools` и др.) через `revit_run_script` без расхода LLM-токенов.
     - Правая колонка — быстрые промпты и встроенный **редактор C#-скриптов** (строгий C# 5.0 для `csc.exe`).
4. **Что делать, если Revit не запущен:** MCP-сервер и `app.html` автоматически перейдут в **режим ожидания (Standby / Offline Preview)** — интерфейс доступен для ознакомления и проверки структуры команд, а при открытии Revit кнопка обновления (`Refresh` / `Обновить`) сразу подключит живую сессию.

---

## Установка и CLI-флаги

- **Интерактивная установка:** запустите `TimscoConnectorSetup_v2.1.0.exe`, выберите язык (`RU` / `EN`), отметьте нужные версии Revit `2022–2027` и нажмите **Установить**.
- **CLI-флаги (без прав администратора):**
  - Тихая установка на русском (по умолчанию): `TimscoConnectorSetup_v2.1.0.exe /S`
  - Тихая установка на английском для выбранных версий Revit: `TimscoConnectorSetup_v2.1.0.exe /S /lang en /versions 2024,2026` (или `/versions all`)
  - Полное удаление (`2022–2027`): `TimscoConnectorSetup_v2.1.0.exe /uninstall /S`
  - Распаковка файлов без установки: `TimscoConnectorSetup_v2.1.0.exe /extract C:\Temp\TimscoPayload`

---

## Сборка из исходников

- **Сборка всех 66 библиотечных DLL (`11 модулей × 6 версий`):**
  ```powershell
  powershell -ExecutionPolicy Bypass -File "scripts\lib\build-lib.ps1" -RevitVersions all
  ```
- **Сборка установщика и релиз-бандла `v2.1.0`:**
  ```powershell
  powershell -ExecutionPolicy Bypass -File "package-release.ps1" -Version "2.1.0"
  ```

---

## Документация

- [docs/ANTIGRAVITY_GUIDE.md](docs/ANTIGRAVITY_GUIDE.md) — подробное руководство по работе с плагином и `app.html` в Google Antigravity
- [scripts/lib/CATALOG.md](scripts/lib/CATALOG.md) — справочник всех `11` DLL-модулей (`49` действий)
- [docs/CHANGELOG.md](docs/CHANGELOG.md) — история изменений (`v1.0.0` – `v2.1.0`)
