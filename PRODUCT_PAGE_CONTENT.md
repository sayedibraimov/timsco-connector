# Timsco Connector v2.0.0 — Документация продукта и релиза (RU / EN)

Документация, архитектура, руководство по установке и справочник модулей **Timsco Connector v2.0.0** для **Autodesk Revit 2022–2027** (разработчик: **ОсОО «Тимско Партнерс» / Timsco Partners LLC**, Бишкек).

---

## 1. Паспорт релиза и артефакты (Verified Build Facts)

- **Продукт / Product:** `Timsco Connector`
- **Версия / Version:** `v2.0.0` (`25.11.2026` / `Nov 25, 2026`)
- **Разработчик / Publisher:** `ОсОО «Тимско Партнерс»` (`Timsco Partners LLC`, г. Бишкек, ул. Шопокова, 121/1, офис 511)
- **Установщик / Installer:** `TimscoConnectorSetup_v2.0.0.exe` (`2219520 bytes` / `2168 KB`)
- **SHA-256:** `f3d8d1420ec1c38f9c9e37593e134dd99ef167da5696a977e30e919f8fd48998`
- **Совместимость / Host Compatibility:** Windows 10 / 11 (x64) · **Autodesk Revit 2022, 2023, 2024, 2025, 2026, 2027** · Google Antigravity IDE / CLI
- **Привилегии / Permissions:** Права администратора (UAC) не требуются — установка в профиль пользователя (`%USERPROFILE%`, `%APPDATA%`, `%LOCALAPPDATA%`, `HKCU`)

---

## 2. Матрица совместимости Autodesk Revit (2022–2027)

| Версии Revit | Целевой TFM / API | Изоляция и особенности |
| :--- | :--- | :--- |
| **2022 – 2023** | `.NET Framework 4.8` (`net48`, `REVIT_LEGACY_ID`) | 32-битный `ElementId.IntegerValue` через `RevitCompat`, изолированный `System.Text.Json.dll` внутри `Addins\<year>\TimscoConnector\` |
| **2024** | `.NET Framework 4.8` (`net48`) | Первый 64-битный `long ElementId.Value`, изолированный `System.Text.Json.dll` внутри `Addins\2024\TimscoConnector\` |
| **2025 – 2026** | `.NET 8.0 Windows` (`net8.0-windows`) | Нативный `AssemblyLoadContext` с выгрузкой сборок без перезапуска Revit и системный `System.Text.Json` |
| **2027** | `.NET 10.0 Windows` (`net10.0-windows`, `REVIT2027`) | Нативный `AssemblyLoadContext` и адаптация API пересечений `Curve.Intersect(..., CurveIntersectResultOption.Detailed)` |

---

## 3. Описание продукта и ключевые возможности (RU / EN)

### Hero (`#overview`)
- **Eyebrow:** `AUTODESK REVIT 2022–2027 · MCP APP · v2.0.0`
- **H1:** `Timsco Connector`
- **RU Lead:** ИИ-моделирование и 0-токеновая автоматизация **Autodesk Revit 2022–2027** в Google Antigravity. Локальный мост без перезапуска Revit, единая транзакция Ctrl+Z и **66 предкомпилированных DLL-модулей** (`11 × 6` версий).
- **EN Lead:** AI-driven modeling and zero-token automation for **Autodesk Revit 2022–2027** in Google Antigravity. Local Named Pipe bridge, single Ctrl+Z transactions, and **66 compiled C# DLL modules** (`11 × 6` Revit versions).
- **KPI-полоса (5 метрик):**
  1. `0` — токенов / вызов DLL (`tokens / DLL call`)
  2. `2022–2027` — 6 версий Revit (`net48`, `net8`, `net10`)
  3. `66` — скомпилированных DLL (`11 модулей × 6 версий`)
  4. `49` — готовых BIM-действий (`BIM actions`)
  5. `2168 KB` — единый локальный установщик (`Single-file EXE`)

### Секция 01 — Архитектура (`#architecture`)
- **RU Заголовок:** Прямой локальный канал между ИИ и Revit 2022–2027 API
- **EN Heading:** Direct Local Channel Between AI and Revit 2022–2027 API
- **Поток (5 узлов):** `Google Antigravity` ↔ `RevitMcpServer.exe (stdio)` ↔ `Windows Named Pipe (rvt-mcp-pipe-<year>)` ↔ `RevitMcpAddin.dll (ExternalEvent)` ↔ `scripts/lib/out/<year>/*.dll`
- **4 ключевых принципа:** Атомарность `Ctrl+Z` (единая транзакция), Единицы `мм` и `градусы` в JSON, Без перезапуска Revit, 100% локальное хранение тяжёлых дампов.

### Секция 02 — Возможности (`#features`)
1. **Мультиверсионный набор 66 DLL (0 токенов) / 66 Multi-Version Zero-Token DLLs:** 11 модулей × 6 версий (`2022–2027`), 49 готовых BIM-операций на C# 5.0 через `RevitCompat` (единицы `мм` и `градусы`, транзакция `Ctrl+Z`).
2. **Timsco CAD Light Studio UI:** Секции `Total Numbers`, `Navigator & Parameters`, `Quick Scripts & Prompts` и вкладка `Families` с живым индикатором подключённой версии Revit (`REVIT 2022–2027`).
3. **Менеджер библиотек семейств `.rfa` / .rfa Family Browser:** Автоопределение каталога года Revit (`RVT <year>\Libraries\English|Russian`), тихая загрузка через `IFamilyLoadOptions`, пакетная расстановка `place_hosted` / `place_unhosted`.
4. **Навык `/image-to-revit` и Quad-рендер:** Воссоздание моделей по референсам и 4-ракурсная верификация `export_quad_png` с откатом `RollBack`.
5. **Аудит BIM, стыки и ламели / BIM QA, Auto-Join & Facades:** Предупреждения, 3D-коллизии, проверка Room, пресет `arch_standard` и DirectShape-ламели `Lover Panel_Mech` (шаг `125 мм`).
6. **Оптимизация квот и портативный EXE / Quota Economy & Portable Setup:** Автосброс `>50` элементов в `%LOCALAPPDATA%\TimscoConnector\out\`, единый установщик `2168 KB`.

---

## 4. Установка и IT-развёртывание

- **Требования:** Windows 10 / 11 (x64) · Autodesk Revit **2022, 2023, 2024, 2025, 2026 или 2027** · Google Antigravity IDE / CLI · Без UAC / прав администратора.
- **4 шага:**
  - `01` Скачать `TimscoConnectorSetup_v2.0.0.exe` (`2168 KB`), сверить `SHA-256`; при предупреждении SmartScreen нажать «Подробнее → Выполнить в любом случае».
  - `02` Закрыть активные окна Revit и запустить установщик. Мастер автоматически обнаружит установленные Revit `2022–2027` (можно скорректировать галочки) и очистит старую `RevitMcpAddin.dll` от `v1.x`.
  - `03` Открыть нужную версию Autodesk Revit (`2022–2027`) и нажать «Всегда загружать» (**Always Load**) для `RevitMcpAddin`.
  - `04` Перезапустить Google Antigravity, вызвать `open_revit_inspector` или выполнить проверку `CleanupTools.dll` (`model_health`).
- **IT CLI-ключи (`installer/TimscoSetup.cs`):**
  - `TimscoConnectorSetup_v2.0.0.exe /S` — тихая автоустановка для всех обнаруженных версий Revit (также `/silent`, `/quiet`)
  - `TimscoConnectorSetup_v2.0.0.exe /S /versions 2024,2026` — тихая установка строго для указанных версий (или `all` для `2022–2027`)
  - `TimscoConnectorSetup_v2.0.0.exe /uninstall /S` — тихое полное удаление плагина, надстроек всех версий `2022–2027` и записи `HKCU`
  - `TimscoConnectorSetup_v2.0.0.exe /extract C:\Temp\TimscoPayload` — распаковка без изменения системы (аудит/GPO)
  - `set TIMSCO_OUT_DIR=D:\BIM\TimscoExports` — переопределение папки выгрузок
- **Структура файлов на ПК:**
  - `%USERPROFILE%\.gemini\config\plugins\timsco-connector\` (`bin\RevitMcpServer.exe`, `ui\app.html`, `scripts\lib\out\<year>\*.dll`, `src\RevitCompat.cs`, `rules\AGENTS.md`, `skills\image-to-revit\SKILL.md`, `assets\`, `plugin.json`, `mcp_config.json`, `UninstallTimscoConnector.exe`)
  - `%APPDATA%\Autodesk\Revit\Addins\<year>\` (`RevitMcpAddin.addin` + папка `TimscoConnector\RevitMcpAddin.dll` с изолированными зависимостями)
  - `%LOCALAPPDATA%\TimscoConnector\out\` (JSON-отчёты, CSV в `schedules\`, PNG в `snapshots\`)

---

## 5. Журнал релизов (кратко)

- **`v2.0.0` (25.11.2026 — Текущий релиз):** Поддержка Revit `2022–2027` (`net48`, `net8.0-windows`, `net10.0-windows`), `RevitCompat`, версионные Named Pipe каналы (`rvt-mcp-pipe-<year>`), **66 DLL** (`11 × 6`), адаптация API Revit 2027, мультиверсионный установщик с автоопределением и флагом `/versions`.
- **`v1.1.0` (24.11.2026):** Единый нативный EXE (`232 KB`), 3D-логотип **T**, CAD Light Studio UI (`Total Numbers`, `Navigator & Parameters`, `Quick Scripts & Prompts`, `Families`), переносимость путей (`TIMSCO_OUT_DIR`), 11 DLL (`49` действий) + `/image-to-revit`.
- **`v1.0.0` (18.11.2026):** Базовый мост `RevitMcpServer` + `RevitMcpAddin` на Named Pipe, 8 MCP-инструментов, `AssemblyLoadContext` hot-loader, базовые модули моделирования.

---

## 6. Справочные документы пакета

- Полный каталог 11 модулей (49 действий): [CATALOG.md](../scripts/lib/CATALOG.md)
- Подробная история изменений: [CHANGELOG.md](CHANGELOG.md)
- Машиночитаемые метаданные релиза: [product-data.json](product-data.json)
