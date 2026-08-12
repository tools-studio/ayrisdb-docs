# Troubleshooting

Every AyrisDB method returns a `Result<T>`. On failure, check `result.Error.Code` (an `AyrisDBErrorCode`) and `result.Error.Message`. The entries below cover each code, in the `ADB_F` series.

## OpenAsync fails and I don't know why

The specific reason is in `result.Error.Code`. The most common causes are covered individually below. Always check `result.IsSuccess` before using `result.Value`, and log `result.Error.Message` — it includes the underlying SQLite error string where one exists.

## DatabaseNotFound (ADB_F001)

**Cause:** The database file doesn't exist yet, and AyrisDB couldn't create it — usually because the parent directory doesn't exist.

**Solution:** Use a path within `Application.persistentDataPath`, or confirm the directory you're targeting already exists.

## DatabaseCorrupt (ADB_F002)

**Cause:** SQLite's integrity check found errors the first time the database was opened.

**Solution:** The database file itself is corrupt. Restore it from a backup, or delete it and let your game recreate it.

## ConnectionFailed (ADB_F003)

**Cause:** Either the native SQLite library failed to load, or SQLite's own open call returned an error. The most common reason for the library failing to load is running on a platform AyrisDB doesn't include a native binary for — see [Features > Platform Handling](features/platform-handling.md) for what's included.

**Solution:** Confirm you're running on a supported platform. If you are, check `result.Error.Message` for the underlying SQLite error string.

## QueryFailed (ADB_F004)

**Cause:** A SQL syntax error, a constraint violation (`UNIQUE`, `NOT NULL`, `FOREIGN KEY`), or the table doesn't exist.

**Solution:** Check `result.Error.Message` for the SQLite error string and the SQL that failed, and confirm the table's schema matches the query. If this only happens in an IL2CPP build and not in the Editor, confirm you're using parameterized SQL (`QueryAsync<T>("WHERE level > ?", 5)`) rather than a LINQ `Where`-clause expression.

## TableNotFound (ADB_F005)

**Cause:** The table for that type hasn't been created yet.

**Solution:** Call `CreateTableAsync<T>()` before querying that type.

## EncryptionKeyInvalid (ADB_F006)

**Cause:** The database is encrypted, and the key you provided failed authentication.

**Solution:** Confirm you're using the correct key. If the database was created without encryption, don't provide a key when opening it.

## EncryptionNotActive (ADB_F007)

**Cause:** A key was provided, but AyrisDB's own guard detected that encryption isn't actually active on the resulting connection.

**Solution:** Confirm `ConnectionOptions.EncryptionKey` is not null and was set before the connection was opened. If it was, this usually means the wrong native library variant loaded.

## PlatformNotSupported (ADB_F009)

**Cause:** Console SQLite support requires the separate AyrisDB Console NDA package, distributed through each platform's developer program. AyrisDB Free doesn't include console native libraries.

**Solution:** Install the console-specific AyrisDB package via your platform's developer program.

## DiskFull (ADB_F010)

**Cause:** The device doesn't have enough free storage for the write.

**Solution:** Prompt the player to free up device storage before retrying.
