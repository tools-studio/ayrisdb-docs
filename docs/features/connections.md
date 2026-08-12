# Connections

## Purpose

Opening a SQLite database on any platform normally means writing your own per-platform path resolution, journal mode selection, and integrity checking. Connections handle all of that for you.

## Workflow

Call `AyrisDB.OpenAsync(filename)` (or the synchronous `AyrisDB.Open`) with a filename. The result is a `Result<IConnection>` — check `IsSuccess` before using `Value`. Dispose the connection (or wrap it in a `using` statement) when you're done with it.

## Configuration

Pass a `ConnectionOptions` instance as the second argument to control encryption, integrity-check policy, and journal mode. See [Configuration](../configuration.md).

## Example

```csharp
var result = await AyrisDB.OpenAsync("game.db");
if (!result.IsSuccess) { Debug.LogError(result.Error.Message); return; }
using var conn = result.Value;
```
