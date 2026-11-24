# Timsco Connector v1.1.0 — Product Page Content & Webflow Handoff Kit

Документация, дизайн-токены и готовая структура контента (**RU / EN**) для публикации страницы продукта **Timsco Connector** на сайте **`timscoarchitects.kg`** (**Webflow**) и в Figma.

---

## 1. Паспорт релиза и артефакты (Verified Build Facts)

- **Продукт / Product:** `Timsco Connector`
- **Версия / Version:** `v1.1.0` (`24.11.2026` / `Nov 24, 2026`)
- **Разработчик / Publisher:** `ОсОО «Тимско Партнерс»` (`Timsco Partners LLC`, г. Бишкек, ул. Шопокова, 121/1, офис 511)
- **Установщик / Installer:** `TimscoConnectorSetup_v1.1.0.exe` (`237 568` bytes / `232 KB`)
- **SHA-256:** `d70f9807fe637634257f2bbf8975eb7dd990ee7ce119de64521397790be53a47`
- **Совместимость / Host Compatibility:** Windows 10 / 11 (x64) · Autodesk Revit 2026 (.NET 8) · Google Antigravity IDE / CLI
- **Привилегии / Permissions:** Права администратора (UAC) не требуются — установка в профиль пользователя (`%USERPROFILE%`, `%APPDATA%`, `%LOCALAPPDATA%`, `HKCU`)

---

## 2. Визуальная система и дизайн-токены (`timscoarchitects.kg` + Timsco Connector)

| Токен / Token | Значение / Value | Назначение / Usage |
| :--- | :--- | :--- |
| **Display Font** | `Urbanist` (weights 300, 400, 500) | Крупные заголовки `H1`, `H2` с плотным трекингом `-0.025em` |
| **Body & UI Font** | `Inter` (weights 400, 500, 600) | Основной текст, навигация в верхнем регистре, кнопки |
| **Technical Mono** | `Inconsolata` | Номера шагов (`01–04`), eyebrow-теги, KPI-подписи, код и SHA-256 |
| **Surface (Light)** | `#FFFFFF` / `#F8F9FB` | Чистый архитектурный фон; тонкая сетка чертежа `48×48 px` (`rgba(0,0,0,0.032)`) |
| **Surface (Dark Hero)** | `#11141B` | Главный тёмный экран (соответствует Hero на `timscoarchitects.kg`) |
| **Primary Ink** | `#1A1A1A` / `#252525` | Заголовки, текст меню, основные подписи на светлом фоне |
| **Muted Ink** | `#6B7280` | Описания карточек, метаданные, подписи |
| **Hairline Border** | `1px solid #DCDCDC` (light) / `rgba(255,255,255,0.16)` (dark) | Вертикальные направляющие (rails) и сетка ячеек без теней |
| **Brand Accent (Cobalt)** | `#004EE0` (hover `#003CB0`) | Главная CTA-кнопка скачивания, активные DLL-бейджи, 3D-логотип **T** |
| **Brand Highlight** | `#669CFF` / `#EFF6FF` | Светлая грань изометрии **T**, тех-бейджи на тёмном фоне, мягкая подсветка |
| **Corner Radius** | `0px` (всюду, кроме иконки SVG `rx="5.5"`) | Строгая прямоугольная геометрия кнопок, карточек, вкладок и таблиц |

---

## 3. Структура и тексты для вёрстки (RU / EN Copy Deck)

### Hero (`#overview`)
- **Eyebrow:** `AUTODESK REVIT 2026 · MCP APP · v1.1.0`
- **H1:** `Timsco Connector`
- **RU Lead:** ИИ-моделирование и 0-токеновая автоматизация Autodesk Revit 2026 в Google Antigravity. Локальный мост без перезапуска Revit, единая транзакция Ctrl+Z и библиотека из 11 горяче-загружаемых DLL-модулей.
- **EN Lead:** AI-driven modeling and zero-token automation for Autodesk Revit 2026 in Google Antigravity. Local Named Pipe bridge with zero Revit restarts, single Ctrl+Z transactions, and 11 hot-load C# DLL modules.
- **KPI-полоса (5 метрик):**
  1. `0` — токенов / вызов DLL (`tokens / DLL call`)
  2. `11` — DLL-модулей (`hot-load DLLs`)
  3. `49` — BIM-действий (`BIM actions`)
  4. `3 237` — семейств `.rfa` (`English 3237 / Russian 29`)
  5. `232 KB` — 100% локально (`Single-file EXE & Named Pipe`)

### Секция 01 — Архитектура (`#architecture`)
- **RU Заголовок:** Прямой локальный канал между ИИ и Revit 2026 API
- **EN Heading:** Direct Local Channel Between AI and Revit 2026 API
- **Поток (5 узлов):** `Google Antigravity` ↔ `RevitMcpServer.exe (stdio)` ↔ `Windows Named Pipe` ↔ `RevitMcpAddin.dll (.NET 8 ExternalEvent)` ↔ `11 Hot-Load DLLs (AssemblyLoadContext)`
- **4 ключевых принципа:** Атомарность `Ctrl+Z` (единая транзакция), Единицы `мм` и `градусы` в JSON, Без перезапуска Revit, 100% локальное хранение тяжёлых дампов.

### Секция 02 — Возможности (`#features`, 6 карточек в сетке 3×2)
1. **Библиотека 11 DLL-модулей (0 токенов) / 11 Hot-Load Zero-Token DLLs:** 49 готовых операций C# 5.0 для Revit 2026 (`ElementId.Value`).
2. **Timsco CAD Light Studio UI:** Секции `Total Numbers`, `Navigator & Parameters`, `Quick Scripts & Prompts` и вкладка `Families`.
3. **Менеджер библиотек семейств `.rfa` / .rfa Family Browser:** Поиск по 3 237 English и 29 Russian `.rfa`, тихая загрузка через `IFamilyLoadOptions`, расстановка `place_hosted` / `place_unhosted`.
4. **Навык `/image-to-revit` и Quad-рендер:** Воссоздание моделей по референсам и 4-ракурсная верификация `export_quad_png` с откатом `RollBack`.
5. **Аудит BIM, стыки и ламели / BIM QA, Auto-Join & Facades:** Предупреждения, 3D-коллизии, проверка Room, пресет `arch_standard` и DirectShape-ламели `Lover Panel_Mech` (шаг `125 мм`).
6. **Оптимизация квот и портативный EXE / Quota Economy & Portable Setup:** Автосброс `>50` элементов в `%LOCALAPPDATA%\TimscoConnector\out\`, единый установщик `232 KB`.

### Секция 03 — Установка и IT-развёртывание (`#install`)
- **Требования (4 ячейки):** `Windows 10 / 11 (x64)` · `Autodesk Revit 2026 (.NET 8)` · `Google Antigravity IDE / CLI` · `Без UAC / прав администратора`
- **4 шага:**
  - `01` Скачать `TimscoConnectorSetup_v1.1.0.exe` (`232 KB`), сверить `SHA-256`; при предупреждении SmartScreen нажать «Подробнее → Выполнить в любом случае».
  - `02` Закрыть Revit 2026 и запустить установку (занимает `< 3 сек`, переносит устаревший плагин `revit-mcp`).
  - `03` Открыть Revit 2026 и нажать «Всегда загружать» (**Always Load**) для `RevitMcpAddin`.
  - `04` Перезапустить Google Antigravity, вызвать `open_revit_inspector` или `CleanupTools.dll` (`model_health`).
- **IT CLI-ключи (`installer/TimscoSetup.cs`):**
  - `TimscoConnectorSetup_v1.1.0.exe /S` — тихая установка (`/silent`, `/quiet`)
  - `TimscoConnectorSetup_v1.1.0.exe /uninstall /S` — тихое удаление
  - `TimscoConnectorSetup_v1.1.0.exe /extract C:\Temp\TimscoPayload` — распаковка без установки для аудита/GPO
  - `set TIMSCO_OUT_DIR=D:\BIM\TimscoExports` — смена папки выгрузок
- **Структура файлов на ПК:**
  - `%USERPROFILE%\.gemini\config\plugins\timsco-connector\` (`bin\RevitMcpServer.exe`, `ui\app.html`, `scripts\lib\out\*.dll`, `rules\AGENTS.md`, `skills\image-to-revit\SKILL.md`, `assets\`, `plugin.json`, `mcp_config.json`, `UninstallTimscoConnector.exe`)
  - `%APPDATA%\Autodesk\Revit\Addins\2026\` (`RevitMcpAddin.addin`, `RevitMcpAddin.dll`)
  - `%LOCALAPPDATA%\TimscoConnector\out\` (JSON-отчёты, CSV в `schedules\`, PNG в `snapshots\`)

### Секция 04 — Релизы (`#releases`)
- **`v1.1.0` (24.11.2026 — Текущий релиз):** Единый нативный EXE (`232 KB`), 3D-логотип **T**, CAD Light Studio UI (`Total Numbers`, `Navigator & Parameters`, `Quick Scripts & Prompts`, `Families`), переносимость путей (`TIMSCO_OUT_DIR`, резолв коротких имён DLL), 11 DLL (`49` действий) + `/image-to-revit`.
- **`v1.0.0` (18.11.2026 — Первый релиз):** Базовый мост `RevitMcpServer` + `RevitMcpAddin` на Named Pipe, 8 MCP-инструментов, `AssemblyLoadContext` hot-loader, базовые модули моделирования.

### Секция 05 — Краткая документация (`#docs`)
- Полный каталог **11 DLL (49 действий)**, таблица **8 MCP-инструментов** (`open_revit_inspector`, `revit_get_status`, `revit_query_elements`, `revit_modify_parameter`, `revit_audit_warnings`, `revit_list_types`, `revit_create_ceilings_from_floors`, `revit_run_script`) и 6 ответов FAQ.
- Источник данных в машинном виде: [product-data.json](product-data.json).

---

## 4. Два способа публикации в Webflow (`timscoarchitects.kg`)

### Вариант А — Быстрая вставка через `<iframe>` (HTML Embed)
1. Опубликуйте [index.html](index.html) на статическом хостинге (GitHub Pages / Cloudflare Pages / Netlify / корпоративный поддомен).
2. Разместите `TimscoConnectorSetup_v1.1.0.exe` на **GitHub Releases**, **Cloudflare R2** или **AWS S3** (Webflow Assets не принимает `.exe`) и пропишите прямую ссылку в `DOWNLOAD_URL` внутри `index.html` (или через `?downloadUrl=...`).
3. Скопируйте код из [webflow-embed.html](webflow-embed.html) в компонент **Code Embed** в Webflow Designer.
   - Параметр `?embed=1` автоматически скроет дублирующую шапку и подвал сайта.
   - Для RU-страницы используйте `?embed=1&lang=ru`, для EN-страницы — `?embed=1&lang=en`.
   - Скрипт `timsco-connector:resize` сам подстраивает высоту `iframe` без двойного скролла.

### Вариант B — Нативная сборка в Webflow Designer + CMS Collections
1. Воссоздайте сетку Client-First (`padding-global` + вертикальные `1px solid #DCDCDC` направляющие) в Webflow Designer, используя токены из раздела 2.
2. Импортируйте готовые таблицы UTF-8 (с BOM) в **Webflow CMS Collections**:
   - **Modules (11 элементов):** [webflow-modules.csv](webflow-modules.csv)
   - **Releases (2 элемента):** [webflow-releases.csv](webflow-releases.csv)
3. Привяжите кнопки скачивания в Webflow к внешней ссылке на `TimscoConnectorSetup_v1.1.0.exe` (GitHub Releases / R2 / S3).
