# Timsco Connector v1.1.0 — Autodesk Revit 2026 MCP App & Zero-Token BIM Suite

Official distribution repository & product documentation for **Timsco Connector** by **Timsco Partners LLC** ([timscoarchitects.kg](https://www.timscoarchitects.kg), Bishkek, Kyrgyzstan).

- **Product Page & Documentation (RU / EN):** [https://sayedibraimov.github.io/timsco-connector/](https://sayedibraimov.github.io/timsco-connector/)
- **Direct Windows Installer Download:** [TimscoConnectorSetup_v1.1.0.exe (d70f9807fe637634257f2bbf8975eb7dd990ee7ce119de64521397790be53a47)](https://github.com/sayedibraimov/timsco-connector/releases/download/v1.1.0/TimscoConnectorSetup_v1.1.0.exe) (232 KB / 237 568 bytes)
- **Latest Release Page:** [v1.1.0](https://github.com/sayedibraimov/timsco-connector/releases/tag/v1.1.0)

## Quick Install (Windows 10/11 x64 + Autodesk Revit 2026)

1. Download **TimscoConnectorSetup_v1.1.0.exe** from [Releases](https://github.com/sayedibraimov/timsco-connector/releases/tag/v1.1.0).
2. Verify checksum if needed: `SHA-256: d70f9807fe637634257f2bbf8975eb7dd990ee7ce119de64521397790be53a47`.
3. Run the installer without Admin/UAC rights (%USERPROFILE% user scope). If Windows SmartScreen appears, select **More info → Run anyway**.
4. Open Autodesk Revit 2026 and click **Always Load** for RevitMcpAddin.

### IT CLI Deployment Flags

- Silent install: `TimscoConnectorSetup_v1.1.0.exe /S`
- Silent uninstall: `TimscoConnectorSetup_v1.1.0.exe /uninstall /S`
- Extract payload for audit/GPO: `TimscoConnectorSetup_v1.1.0.exe /extract C:\Temp\TimscoPayload`