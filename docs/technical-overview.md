# Technical Overview

The control center is a .NET 8 WPF application managing emulator discovery, task import and state, logs, configuration, and per-instance worker status.

LDPlayer is controlled through its console interface and ADB bridge. Vivaldi readiness is treated as application state rather than a fixed delay. First-run pages and Android permission states are handled before task claim.

Automation uses Android accessibility and UI state inspection with bounded actions. Workflow logic is separated from lifecycle and persistence so failure semantics can be tested independently.

Workers are isolated per emulator. Shared UI-dump access is serialized where needed to reduce cross-instance contention.
