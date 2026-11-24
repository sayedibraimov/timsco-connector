# Timsco Connector v2.0.0 — Autodesk Revit 2022–2027 MCP App & Zero-Token BIM Suite

Official distribution repository & documentation for **Timsco Connector** by **Timsco Partners LLC** ([timscoarchitects.kg](https://www.timscoarchitects.kg), Bishkek, Kyrgyzstan).

- **Product Page & Documentation (RU / EN):** [https://sayedibraimov.github.io/timsco-connector/](https://sayedibraimov.github.io/timsco-connector/)
- **Direct Windows Installer Download:** [TimscoConnectorSetup_v2.0.0.exe (2168 KB / 2219520 bytes)](https://github.com/sayedibraimov/timsco-connector/releases/download/v2.0.0/TimscoConnectorSetup_v2.0.0.exe)
- **Installer SHA-256:** `f3d8d1420ec1c38f9c9e37593e134dd99ef167da5696a977e30e919f8fd48998`
- **Latest Release:** [v2.0.0](https://github.com/sayedibraimov/timsco-connector/releases/tag/v2.0.0)
- **Docs in Repo:** [CHANGELOG.md](CHANGELOG.md) · [CATALOG.md (11 Modules / 49 Actions)](CATALOG.md) · [PRODUCT_PAGE_CONTENT.md](PRODUCT_PAGE_CONTENT.md)

## Supported Autodesk Revit Versions

| Revit Versions | Target TFM / API | Runtime & Isolation |
|---|---|---|
| **2022 – 2023** | `net48` (`REVIT_LEGACY_ID`) | `ElementId.IntegerValue` via `RevitCompat`, isolated `System.Text.Json.dll` in `TimscoConnector\` |
| **2024** | `net48` (64-bit `ElementId.Value`) | First 64-bit ID on .NET Framework 4.8, isolated runtime in `TimscoConnector\` |
| **2025 – 2026** | `net8.0-windows` | Native collectible `AssemblyLoadContext` hot-loader |
| **2027** | `net10.0-windows` (`REVIT2027`) | Native `AssemblyLoadContext` + `CurveIntersectResultOption.Detailed` |

## Quick Install (Windows 10/11 x64 · No Admin / UAC Required)

1. Download **TimscoConnectorSetup_v2.0.0.exe** from [Releases](https://github.com/sayedibraimov/timsco-connector/releases/tag/v2.0.0).
2. Verify checksum if needed: `SHA-256: f3d8d1420ec1c38f9c9e37593e134dd99ef167da5696a977e30e919f8fd48998`.
3. Run the installer in user scope (`%USERPROFILE%`). It auto-detects installed Revit **2022–2027** versions and cleans up legacy `v1.x` files. If Windows SmartScreen appears, select **More info → Run anyway**.
4. Open Autodesk Revit (2022–2027) and click **Always Load** for **RevitMcpAddin**.

### IT & Enterprise CLI Deployment Flags

- Silent auto-detect install (2022–2027): `TimscoConnectorSetup_v2.0.0.exe /S`
- Silent targeted versions: `TimscoConnectorSetup_v2.0.0.exe /S /versions 2024,2026` (or `all`)
- Silent full uninstall across all versions: `TimscoConnectorSetup_v2.0.0.exe /uninstall /S`
- Extract payload for audit/GPO: `TimscoConnectorSetup_v2.0.0.exe /extract C:\Temp\TimscoPayload`