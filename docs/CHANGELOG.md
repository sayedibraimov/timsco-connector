# Changelog — Timsco Connector

Все значимые изменения продукта **Timsco Connector** (Timsco Partners LLC, Бишкек) фиксируются в этом документе.

---

## [v2.1.0] — 2026-11-25

**Полная русская локализация (по умолчанию) + выбор языка RU / EN и руководство по работе в Google Antigravity.**

- **Installer:** `TimscoConnectorSetup_v2.1.0.exe` (`2242560 bytes` / `2190 KB`)
- **SHA-256:** `cdb4873ae6b9da54ef1ee25e32c295ca32884290fb8724a4cefa2c4edf2285ce`

### Добавлено / Added
- **Выбор языка при установке (`TimscoConnectorSetup_v2.1.0.exe`):**
  - В шапке установщика добавлены кнопки **`RU`** (по умолчанию) и **`EN`** с мгновенным переключением всех надписей, описаний и отчётов установки/удаления.
  - Новый CLI-флаг `/lang ru|en` для сценариев тихой установки `/S`.
  - Сохранение выбранного языка в `config.json` (`{"language":"ru"|"en"}`) и генерация локализованного `plugin.json`.
- **Переключатель `RU | EN` в интерфейсе `ui/app.html`:**
  - Все вкладки, метрики, фильтры, таблица навигатора, модальные окна, 22 кнопки прямых DLL-инструментов, 7 шаблонов ИИ-задач, советы по экономии квот, уведомления и C#-редактор переведены на русский язык с живым переключением на английский (`localStorage["timsco_ui_lang"]`).
  - Динамическая подстановка активного года Revit (`2022–2027`) в путях к библиотекам `.rfa`.
- **Локализация MCP-сервера (`RevitMcpServer.exe`):**
  - Поддержка `config.json` и переменной окружения `TIMSCO_LANG=ru|en` (по умолчанию `ru`).
  - Переведены описания `tools/list`, `resources/list`, сводки `Standby` и `FormatLiveSummary`, а также автоматическая передача начального языка `__TIMSCO_LANG__` в `ui/app.html`.
- **Руководство для Google Antigravity (`docs/ANTIGRAVITY_GUIDE.md`):**
  - Добавлена подробная инструкция по установке на RU/EN, открытию `app.html` через `open_revit_inspector` / боковую панель `aux_pane`, работе с `0`-токеновыми кнопками и обходу типичных ошибок.

### Изменено / Changed
- Из дистрибутива и GitHub-репозитория исключена веб-страница продукта (`index.html`, `product-data.json`, `PRODUCT_PAGE_CONTENT.md`); фокус публикации — документация репозитория и бинарный релиз.
- Сборка `RevitMcpServer.exe` и `TimscoConnectorSetup` переведена на `/codepage:65001 /utf8output` для гарантированной чистоты UTF-8 кириллицы независимо от системной локали Windows.

---

## [v2.0.0] — 2026-11-25

**Мультиверсионный релиз для Autodesk Revit 2022–2027.** Единая кодовая база и единый Windows-установщик с автоматическим определением установленных версий Revit и изоляцией зависимостей.

- **Installer:** `TimscoConnectorSetup_v2.0.0.exe` (`2219520 bytes` / `2168 KB`)
- **SHA-256:** `f3d8d1420ec1c38f9c9e37593e134dd99ef167da5696a977e30e919f8fd48998`

### Добавлено / Added
- **Поддержка Autodesk Revit 2022–2027** (6 целевых версий):
  - `2022–2023`: `.NET Framework 4.8` (`net48` + `REVIT_LEGACY_ID` / `ElementId.IntegerValue`), изолированные зависимости в `TimscoConnector\`.
  - `2024`: `.NET Framework 4.8` (`net48`), 64-битный `long ElementId.Value`, изолированные зависимости.
  - `2025–2026`: `.NET 8.0 Windows` (`net8.0-windows`), нативный `AssemblyLoadContext`.
  - `2027`: `.NET 10.0 Windows` (`net10.0-windows`), адаптация нового API пересечений `Curve.Intersect(..., CurveIntersectResultOption.Detailed)`.
- **Хелпер совместимости `src\RevitCompat.cs`:** единые вызовы `IdValue(ElementId)`, `MakeId(long)` и `GetRevitYear(UIApplication)` во всём коде надстройки и 11 модулей библиотеки (`66` скомпилированных DLL: `11 модулей × 6 версий`).
- **Версионно-изолированные Named Pipe каналы:** `\\.\pipe\rvt-mcp-pipe-<year>` с автоматическим fallback на `\\.\pipe\rvt-mcp-pipe`, позволяющие одновременно работать с несколькими версиями Revit на одной рабочей станции.
- **Умный установщик `TimscoConnectorSetup_v2.0.0.exe`:**
  - Автоопределение установленных версий Revit (`2022–2027`) и интерактивные чекбоксы выбора версий в GUI.
  - Новый CLI-ключ `/versions 2024,2026` (или `/versions all`) для сценариев `/S`.
  - Автоматическая очистка устаревшей плоской `RevitMcpAddin.dll` при обновлении с `v1.x` и полное удаление по всем версиям при `/uninstall`.
- **Динамическое определение года в `FamilyTools.dll` (`scan_rfa_files`):** автовыбор каталога `C:\ProgramData\Autodesk\RVT <year>\Libraries\Russian|English` по активной сессии Revit.

### Изменено / Changed
- Сборка библиотеки и надстройки переведена на SDK-стили MSBuild (`build\RevitLibModule.csproj`, `build\RevitMcpAddin.csproj`, `build\Directory.Build.props`) с официальными reference-сборками `Nice3point.Revit.Api`.
- Упрощена структура дистрибутива и документации: удалены устаревшие шаблоны стороннего встраивания; основной канал публикации — GitHub Releases и автономная страница продукта.

---

## [v1.1.0] — 2026-11-24

- Единый нативный Windows-установщик `TimscoConnectorSetup_v1.1.0.exe` (`232 KB`) без UAC/прав администратора, с тихими ключами `/S`, `/uninstall /S`, `/extract` и регистрацией в «Приложениях Windows».
- Фирменная айдентика Timsco Partners LLC (изометрический 3D-логотип **T**).
- Редизайн интерфейса **Timsco CAD Light Studio UI** (`ui/app.html`): сворачиваемые секции **Total Numbers**, **Navigator & Parameters**, вкладка **Families** (`.rfa`), **Quick Scripts & Prompts**.
- 100% переносимость между ПК (`%LOCALAPPDATA%\TimscoConnector\out\`, переменная `TIMSCO_OUT_DIR`, резолв коротких имён DLL из `scripts\lib\out\`).
- Завершён комплект из 11 горяче-загружаемых C#-модулей (`49` действий) + навык `/image-to-revit`.

---

## [v1.0.0] — 2026-11-18

- Базовый MCP-сервер `RevitMcpServer.exe` (`stdio JSON-RPC 2.0`) и надстройка `RevitMcpAddin.dll` (.NET 8 / Revit 2026 API) с маршалингом в главный поток через `ExternalEvent`.
- Инструмент `revit_run_script` с горячей загрузкой C#-сборок в `AssemblyLoadContext` без перезапуска Revit.
- Стартовые 8 MCP-инструментов и базовые модули моделирования (`DatumTools`, `ArchModelTools`, `StructModelTools`, `FamilyTools`, `ElementOpsTools`, `ViewTools`, `LouverPanelTools`).
