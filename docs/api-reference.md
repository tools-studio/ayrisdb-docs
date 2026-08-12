# API Reference

All types in the `ToolsStudio.AyrisDB` namespace unless noted.

## AyrisDB (static class)

**`Version`** → `string`
The product's own version string, e.g. `"1.0.0"`.

**`SQLiteVersion`** → `string`
The compiled-in SQLite library's version, queried at runtime.

**`OpenAsync(filename, options, ct)`** → `Task<Result<IConnection>>`
Opens or creates a SQLite database. `filename` is relative to the platform database directory (resolved by `Platform.ResolveDatabasePath`). `options` is optional; null uses defaults. Runs integrity check on first open per `ConnectionOptions.EnableIntegrityCheck`. Throws `DatabaseCorruptException` if integrity check fails. Returns `ADB_F003` if the native library fails to load.

**`Open(filename, options)`** → `Result<IConnection>`
Synchronous variant. Blocks the calling thread. Do not call from the Unity main thread.

**`AyrisDB.EncryptionKey.Derive(passphrase)`** → `string`
Derives an encryption key from `passphrase`. The returned string is passed to `ConnectionOptions.EncryptionKey`. Platform-native key management (Keychain, Keystore) is in AyrisDB Pro.

## IConnection (interface)

**Properties:**
- `string DatabasePath` — absolute path to the database file
- `bool IsOpen` — false after `Dispose()`
- `int SchemaVersion` — current `PRAGMA user_version`; always 0 in Free (written only by Pro's migration runner)

**Async methods** (all return `Task<Result<T>>`; all accept optional `CancellationToken ct = default`):

| Method | T | Notes |
|---|---|---|
| `CreateTableAsync<T>()` | `int` | Returns 0 (table existed) or 1 (table created) |
| `InsertAsync<T>(item)` | `int` | Returns rows inserted (always 1 on success) |
| `InsertOrReplaceAsync<T>(item)` | `int` | Replaces on PrimaryKey conflict |
| `UpdateAsync<T>(item)` | `int` | Matches on PrimaryKey; returns rows updated |
| `DeleteAsync<T>(item)` | `int` | Matches on PrimaryKey; returns rows deleted |
| `QueryAsync<T>(sql, args)` | `List<T>` | `sql` is the WHERE clause or full SELECT; use `?` for parameters |
| `ExecuteAsync(sql, args)` | `int` | Rows affected |
| `ExecuteScalarAsync<T>(sql, args)` | `T` | First column of first row |
| `FindAsync<T>(primaryKey)` | `T` | Returns ADB_F005 if not found |
| `CountAsync<T>()` | `int` | Row count for T's table |

**Synchronous variants:** Same name without `Async` suffix, no `CancellationToken`, return `Result<T>` (not `Task<Result<T>>`).

**Dispose:** Drains the work queue, closes the sqlite3 handle. After Dispose, all operations return `ADB_F003`.

## ConnectionOptions (class)

| Property | Type | Default | Notes |
|---|---|---|---|
| `EncryptionKey` | `string` | `null` | Null = no encryption |
| `EnableIntegrityCheck` | `IntegrityCheckPolicy` | `FirstOpen` | `Always`, `FirstOpen`, `Never` |
| `JournalMode` | `JournalMode` | `Auto` | `Auto` uses platform default |

## ORM attributes

| Attribute | Usage | Notes |
|---|---|---|
| `[Table(name?)]` | Class | Optional name override |
| `[PrimaryKey]` | Property | Marks the primary key column |
| `[AutoIncrement]` | Property | INTEGER PRIMARY KEY AUTOINCREMENT |
| `[Column(name?)]` | Property | Optional column name override |
| `[Ignore]` | Property | Excluded from all SQL operations |
| `[Unique]` | Property | UNIQUE constraint on column |
| `[Indexed]` | Property | CREATE INDEX on the column |
| `[NotNull]` | Property | NOT NULL constraint |

## Result\<T\> (readonly struct)

| Member | Type | Notes |
|---|---|---|
| `IsSuccess` | `bool` | True when the operation succeeded |
| `Value` | `T` | Valid when IsSuccess is true; default(T) otherwise |
| `Error` | `AyrisDBError` | Valid when IsSuccess is false |
| `Ok(value)` | `static Result<T>` | Creates a success result |
| `Fail(error)` | `static Result<T>` | Creates a failure result |

## ITypeConverter\<T\> (interface)

```csharp
public interface ITypeConverter<T>
{
    string ToStorageValue(T value);
    T      FromStorageValue(string stored);
}
```

Register custom converters: `AyrisDB.TypeConverters.Register<MyType>(new MyTypeConverter())`. Call before the first `OpenAsync`.

## AyrisDBErrorCode (enum)

See [Troubleshooting](troubleshooting.md) for the full symptom/cause/solution mapping for each code.
