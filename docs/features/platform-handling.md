# Platform Handling

## Purpose

Removes the need for `#if` platform-specific branches in your own code for database paths and journal mode selection. AyrisDB's native binaries cover Windows (x64, x86), Linux (x64), and WebGL.

## Workflow

Call `AyrisDB.OpenAsync(filename)` as you would on any platform — the correct path and journal mode are selected for you automatically.

## Configuration

`ConnectionOptions.JournalMode` defaults to `Auto`, which uses the platform's default. Set it explicitly if you need to override it.

## Platform notes

**Windows.** Database path resolves under `Application.persistentDataPath` (`AppData\LocalLow` on Windows). Journal mode is WAL. No special setup is required.

**Linux.** Database path resolves under `Application.persistentDataPath` (`~/.config/unity3d` on Linux). Journal mode is WAL. No special setup is required.

**WebGL.** The database path is a fixed IDBFS mount point, computed from `Application.productName` at connection open time — do not use `Application.persistentDataPath` directly for WebGL, since Unity's default path includes a build-specific subfolder that changes on every new build upload. Journal mode is DELETE, since WAL requires shared-memory-mapped files not available in the browser sandbox. Data is stored in IDBFS (Emscripten's IndexedDB-backed filesystem); an explicit `FS.syncfs()` call runs after every committed transaction so writes survive page reloads. WebGL has no multi-threading — all database operations run synchronously on the main thread. Avoid queries returning more than 1,000 rows per call, and index frequently-queried columns.
