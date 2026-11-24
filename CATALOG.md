# Timsco Connector Base Script Library (`scripts/lib`, Revit 2022–2027)

Готовая библиотека переиспользуемых C#-модулей для моделирования элементов, зданий и вставки семейств в **Autodesk Revit 2022–2027** через `revit_run_script` (горячая загрузка в память без перезапуска Revit).

- **Единицы измерения:** все координаты, длины, отметки и смещения — в **миллиметрах (мм)**; углы поворота — в **градусах**.
- **Транзакции:** каждая операция изменения модели выполняется в одной атомарной транзакции (`Ctrl+Z` отменяет всю команду целиком).
- **Мультиверсионная сборка (66 DLL = 11 модулей × 6 версий):** `powershell -ExecutionPolicy Bypass -File "scripts\lib\build-lib.ps1" -RevitVersions all`. `RevitMcpServer.exe` автоматически выбирает сборку из `scripts\lib\out\<year>\` (с fallback в `scripts\lib\out\`).
- **Совместимость ID:** в исходниках используется `RevitCompat.IdValue` / `RevitCompat.MakeId` (`IntegerValue` на 2022–2023, 64-bit `Value` на 2024–2027).
- **Папка отчётов и PNG по умолчанию:** `%LOCALAPPDATA%\TimscoConnector\out\` (переопределяется переменной `TIMSCO_OUT_DIR`).

---

## Модуль 1: `DatumTools.dll` — Уровни и сетки осей
- **Исходник:** `scripts\lib\DatumTools.cs`
- **Сборка (`assemblyPath`):** `DatumTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `list` | Список всех уровней (по отметке) и осей с координатами. | `{ "action": "list" }` |
| `create_levels` | Создание уровней (+ опционально планов этажей). Пропускает существующие в пределах `5` мм (если не `force: true`). | `{ "action": "create_levels", "levels": [{"name": "L1", "elevationMm": 0}, {"name": "L2", "elevationMm": 3600}], "createFloorPlans": true, "force": false }` |
| `create_grids` | Создание осей явно (`grids`) или параметрической прямоугольной сеткой (`xSpacingsMm`, `ySpacingsMm`). | `{ "action": "create_grids", "xSpacingsMm": [6000, 6000, 6000], "ySpacingsMm": [6000, 6000], "originX": 0, "originY": 0, "overhangMm": 2000 }` |

---

## Модуль 2: `ArchModelTools.dll` — Стены, перекрытия, потолки, шахты, помещения
- **Исходник:** `scripts\lib\ArchModelTools.cs`
- **Сборка (`assemblyPath`):** `ArchModelTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `list_types` | Типоразмеры системных семейств (с толщиной в мм). | `{ "action": "list_types", "category": "Walls" }` (`Walls`, `Floors`, `Ceilings`, `Roofs`, `All`) |
| `create_walls` | Создание стен по массиву `walls` или по контуру `polyline` (`closed: true/false`). | `{ "action": "create_walls", "polyline": [[0,0],[12000,0],[12000,8000],[0,8000]], "closed": true, "level": "Ground Floor", "heightMm": 3800, "wallType": "Типовой - 200мм", "structural": false }` |
| `create_floors` | Создание перекрытий по `points` или прямоугольнику `minX..maxY` (+ внутренние проёмы `openings`). | `{ "action": "create_floors", "floors": [{"level": "Ground Floor", "floorType": "Типовой 300мм", "minX": 0, "minY": 0, "maxX": 12000, "maxY": 8000, "openings": [[[4000,2000],[7000,2000],[7000,5000],[4000,5000]]]}] }` |
| `create_ceilings` | Создание потолков по эскизам с высотой от уровня (`offsetMm`, по умолчанию `2700`). | `{ "action": "create_ceilings", "ceilings": [{"level": "Ground Floor", "offsetMm": 2700, "minX": 0, "minY": 0, "maxX": 12000, "maxY": 8000}] }` |
| `create_shaft_opening` | Создание вертикального проёма шахты между уровнями `baseLevel` и `topLevel`. | `{ "action": "create_shaft_opening", "baseLevel": "B1", "topLevel": "49", "minX": 4000, "minY": 2000, "maxX": 7000, "maxY": 5000 }` |
| `create_rooms` | Размещение помещений в замкнутых контурах по координатам `(x, y)`. | `{ "action": "create_rooms", "rooms": [{"level": "Ground Floor", "x": 2000, "y": 2000, "name": "Офис", "number": "101"}] }` |

---

## Модуль 3: `StructModelTools.dll` — Колонны и балки
- **Исходник:** `scripts\lib\StructModelTools.cs`
- **Сборка (`assemblyPath`):** `StructModelTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `list_types` | Список загруженных типоразмеров несущих/архитектурных колонн и балок. | `{ "action": "list_types" }` |
| `create_columns` | Расстановка колонн явно (`columns`) или автоматически на всех пересечениях осей (`atGridIntersections: true`). | `{ "action": "create_columns", "atGridIntersections": true, "baseLevel": "B1", "topLevel": "Ground Floor", "typeName": "300 x 300", "rotationDeg": 0 }` |
| `create_beams` | Создание балок по начальной/конечной точкам на уровне. | `{ "action": "create_beams", "beams": [{"x1": 0, "y1": 0, "x2": 6000, "y2": 0, "level": "2 Floor", "zOffsetMm": 0, "typeName": ""}] }` |

---

## Модуль 4: `FamilyTools.dll` — Поиск `.rfa`, загрузка и вставка семейств
- **Исходник:** `scripts\lib\FamilyTools.cs`
- **Сборка (`assemblyPath`):** `FamilyTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `scan_rfa_files` | Поиск файлов `.rfa` на диске с автоопределением года Revit (`Libraries\Russian` — `29` файлов; полная библиотека находится в `C:\ProgramData\Autodesk\RVT <year>\Libraries\English`). | `{ "action": "scan_rfa_files", "folder": "C:\\ProgramData\\Autodesk\\RVT 2026\\Libraries\\English", "query": "Door", "limit": 20 }` |
| `list_loaded` | Список загруженных в проект `FamilySymbol` с `placementType`. | `{ "action": "list_loaded", "category": "Doors", "search": "", "limit": 60 }` |
| `load_family` | Загрузка `.rfa` в проект (`IFamilyLoadOptions` без диалогов) и активация типоразмеров. | `{ "action": "load_family", "path": "C:\\ProgramData\\Autodesk\\RVT 2026\\Libraries\\Russian\\...\\M_Стол.rfa", "activate": true }` |
| `place_unhosted` | Вставка свободных семейств (мебель, оборудование, сантехника, светильники и др.) по `(x, y, zMm)` и `rotationDeg`. | `{ "action": "place_unhosted", "instances": [{"familyType": "Стол", "x": 3000, "y": 4000, "zMm": 0, "level": "Ground Floor", "rotationDeg": 90, "parameters": {"Комментарии": "Офис"}}] }` |
| `place_hosted` | Вставка семейств с основой (`hostId` стены/перекрытия): двери, окна, розетки. Позиция по доле стены `t` (`0..1`) или проекции `(x, y)`. | `{ "action": "place_hosted", "instances": [{"familyType": "Одиночная", "hostId": 123456, "t": 0.5, "sillHeightMm": 0, "flipHand": false, "flipFacing": false}] }` |

---

## Модуль 5: `ElementOpsTools.dll` — Трансформации, копирование, инспекция и удаление
- **Исходник:** `scripts\lib\ElementOpsTools.cs`
- **Сборка (`assemblyPath`):** `ElementOpsTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `inspect_elements` | Подробная инспекция (геометрия `location`, `bbox`, до `40` параметров) по `ids` или текущему выделению в Revit (`useSelection: true`). | `{ "action": "inspect_elements", "ids": [364612] }` |
| `select_in_ui` | Выделение указанных элементов в интерфейсе Revit. | `{ "action": "select_in_ui", "ids": [364612] }` |
| `move` | Сдвиг элементов на `(dxMm, dyMm, dzMm)`. | `{ "action": "move", "ids": [12345], "dxMm": 1000, "dyMm": 0, "dzMm": 0 }` |
| `rotate` | Поворот элементов вокруг вертикальной оси `(cxMm, cyMm)` на `angleDeg`. | `{ "action": "rotate", "ids": [12345], "cxMm": 0, "cyMm": 0, "angleDeg": 90 }` |
| `copy_to_levels` | Копирование элементов с `sourceLevel` на список `targetLevels` или на все вышележащие уровни (`allLevelsAbove: true`). | `{ "action": "copy_to_levels", "ids": [12345], "sourceLevel": "Ground Floor", "targetLevels": ["2 Floor", "3 Floor"] }` |
| `delete` | Удаление элементов по ID. | `{ "action": "delete", "ids": [12345] }` |

---

## Модуль 6: `ViewTools.dll` — Виды, PNG-снимки, 3D-камера и референс-подложки
- **Исходник:** `scripts\lib\ViewTools.cs`
- **Сборка (`assemblyPath`):** `ViewTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `list_views` | Список экспортируемых видов (`ThreeD`, `FloorPlan`, `Elevation`, `Section`). | `{ "action": "list_views", "viewType": "ThreeD", "limit": 40 }` |
| `export_view_png` | Экспорт активного или заданного вида в PNG для визуальной самопроверки. | `{ "action": "export_view_png", "viewName": "{3D}", "pixelSize": 1280, "fileName": "latest.png" }` |
| `set_3d_camera` | Установка ракурса 3D-вида по пресету (`iso_se`, `iso_sw`, `iso_ne`, `iso_nw`, `front`, `back`, `right`, `left`, `top`) или `(eyeMm, targetMm)`. | `{ "action": "set_3d_camera", "preset": "iso_se", "focusIds": [12345], "displayStyle": "ShadingWithEdges" }` |
| `import_reference_image` | Загрузка референс-изображения как подложки в 2D-вид (`FloorPlan`/разрез/фасад). | `{ "action": "import_reference_image", "imagePath": "C:\\path\\plan.png", "level": "Ground Floor", "widthMm": 12000 }` |
| `section_box_selection` | Подрезка 3D-вида (`SectionBox`) по выделению в Revit или `elementIds` с отступом `paddingMm`, наведением камеры и `ZoomToFit`. | `{ "action": "section_box_selection", "paddingMm": 1500, "preset": "iso_se" }` |
| `reset_section_box` | Снятие `SectionBox` в 3D-виде (`IsSectionBoxActive = false`) с опциональным `ZoomToFit` и пресетом камеры. | `{ "action": "reset_section_box", "preset": "iso_se", "zoomToFit": true }` |
| `export_quad_png` | Недеструктивный мультиракурсный рендер 4-х PNG (`iso_se`, `iso_nw`, `front`, `top`) через откатываемую транзакцию (`RollBack`) без изменения камеры пользователя. | `{ "action": "export_quad_png", "pixelSize": 1024 }` |

---

## Модуль 7: `LouverPanelTools.dll` — Решетчатые панели (`Lover Panel_Mech`) для непрямоугольных ячеек витража
- **Исходник:** `scripts\lib\LouverPanelTools.cs`
- **Сборка (`assemblyPath`):** `LouverPanelTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `inspect` | Поиск непрямоугольных/угловых панелей на наклонных витражных системах и сверка фазировки ламелей с `Lover Panel_Mech`. | `{ "action": "inspect", "hostIds": [365359, 474462], "autoMechanicalRow": true }` |
| `apply` | Генерация подрезанных 3D-ламелей (DirectShape `OST_CurtainWallPanels`, шаг `125 мм`) с заменой исходной ячейки на `Пусто` (в одной транзакции). Поддерживает `autoMechanicalRow`, `panelIds` или `useSelection: true`. | `{ "action": "apply", "hostIds": [365359, 474462], "autoMechanicalRow": true }` |
| `revert` | Удаление созданных DirectShape-ламелей и восстановление прежних типоразмеров панелей витража (`all: true`, `hostIds`, `panelIds` или `useSelection: true`). | `{ "action": "revert", "all": true }` |

---

## Модуль 8: `AuditTools.dll` — Предупреждения, помещения и коллизии (0 токенов)
- **Исходник:** `scripts\lib\AuditTools.cs`
- **Сборка (`assemblyPath`):** `AuditTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `warnings_summary` | Группировка предупреждений по типу и частоте; при `>20` предупреждений полный JSON в `scripts\out\warnings_audit.json`. | `{ "action": "warnings_summary", "topN": 20 }` |
| `select_warning` | Выделение в Revit элементов по `groupIndex` или подстроке `query`. | `{ "action": "select_warning", "groupIndex": 0, "zoom": true }` |
| `check_rooms` | Проверка `Room`: `unplaced`, `redundant`, `not_enclosed`, дубли номеров; при `>50` — `scripts\out\rooms_audit.json`. | `{ "action": "check_rooms", "selectIssues": false }` |
| `delete_unplaced_rooms` | Удаление неразмещённых помещений (`Location == null`) в одной транзакции. | `{ "action": "delete_unplaced_rooms" }` |
| `clash_check` | Проверка 3D-коллизий между `categoryA` и `categoryB` (`ElementIntersectsElementFilter`), исключая соединённые пары по умолчанию. | `{ "action": "clash_check", "categoryA": "Walls", "categoryB": "Columns", "limit": 100 }` |

---

## Модуль 9: `ParamTools.dll` — Параметры, автонумерация и экспорт спецификаций (0 токенов)
- **Исходник:** `scripts\lib\ParamTools.cs`
- **Сборка (`assemblyPath`):** `ParamTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `audit_params` | Аудит заполнения (`Mark`, `Fire Rating`, `Comments` и др. на экземпляре/типе) с поиском пустых и дублей; при `>50` — `scripts\out\params_audit.json`. | `{ "action": "audit_params", "category": "Doors", "parameter": "Mark", "selectEmpty": false }` |
| `auto_renumber` | Автонумерация выделенных элементов (или категории при `useSelection: false`): уровень → X (шаг 500 мм) → Y → ID. | `{ "action": "auto_renumber", "useSelection": true, "parameter": "Mark", "prefix": "Д-", "startNumber": 1 }` |
| `sync_params` | Перенос уровня (`level_to_comments`), помещения (`room_to_comments`) или копирование параметра (`copy_param`). | `{ "action": "sync_params", "mode": "level_to_comments", "category": "Doors", "targetParam": "Comments" }` |
| `export_schedules` | Экспорт `ViewSchedule` (или сводной ведомости категорий / категории при отсутствии `ViewSchedule`) в `;`-CSV с UTF-8 BOM в `scripts\out\schedules\`. | `{ "action": "export_schedules" }` |

---

## Модуль 10: `JoinTools.dll` — Соединение геометрии и приоритет резки (0 токенов)
- **Исходник:** `scripts\lib\JoinTools.cs`
- **Сборка (`assemblyPath`):** `JoinTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `inspect_joins` | Анализ стыков между `categoryA` и `categoryB` (или выделением): корректные, неверный порядок, не соединены. | `{ "action": "inspect_joins", "categoryA": "Floors", "categoryB": "Walls", "limit": 200 }` |
| `auto_join` | Соединение и выравнивание порядка резки (`arch_standard`: перекрытия>стены, колонны>стены, балки>стены, перекрытия>колонны) в одной транзакции. | `{ "action": "auto_join", "preset": "arch_standard", "limit": 300 }` |
| `unjoin_selection` | Безопасное разъединение всех геометрий у выделенных или переданных `elementIds`. | `{ "action": "unjoin_selection" }` |

---

## Модуль 11: `CleanupTools.dll` — Здоровье модели, чистка графики и видов (0 токенов)
- **Исходник:** `scripts\lib\CleanupTools.cs`
- **Сборка (`assemblyPath`):** `CleanupTools.dll`

| Action | Описание | Параметры (`scriptArgs`) |
| :--- | :--- | :--- |
| `model_health` | Быстрый чек-лист здоровья модели: предупреждения, CAD-импорты/связи, RVT-связи, In-Place, DirectShape, группы, виды/листы, фильтры + `scripts\out\model_health.json`. | `{ "action": "model_health" }` |
| `reset_view_overrides` | Сброс ручных переопределений графики элементов на активном или указанном виде. | `{ "action": "reset_view_overrides" }` |
| `list_unused_views` | Список видов, не размещённых на листах (защищены `{3D}`, активный вид, шаблоны). | `{ "action": "list_unused_views", "includeSchedules": false }` |
| `purge_unused_views` | Безопасное удаление неиспользуемых видов (`dryRun: true` по умолчанию — только предпросмотр). | `{ "action": "purge_unused_views", "dryRun": true }` |

---

## Пример вызова через MCP (`revit_run_script`)

```json
{
  "assemblyPath": "FamilyTools.dll",
  "scriptArgs": {
    "action": "scan_rfa_files",
    "query": "двер",
    "limit": 10
  }
}
```
