# Configuration

AyrisDB's connection behavior is controlled through `ConnectionOptions`, passed as the second argument to `AyrisDB.OpenAsync` or `AyrisDB.Open`. Passing `null` (or omitting it) uses the defaults below.

```csharp
var options = new ConnectionOptions
{
    EncryptionKey = AyrisDB.EncryptionKey.Derive("my-passphrase"),
    JournalMode = JournalMode.WAL
};

var result = await AyrisDB.OpenAsync("game.db", options);
```

## Encryption

`EncryptionKey` defaults to `null` (no encryption). Derive a key from a passphrase with `AyrisDB.EncryptionKey.Derive(passphrase)`. Once a database is created with a key, the same key must be supplied on every subsequent open. See [Features > Encryption](features/encryption.md) for how the encryption guard works.

## Journal mode

`JournalMode` defaults to `Auto`, which uses the platform's default (WAL on Windows and Linux, DELETE on WebGL, since WAL requires shared-memory-mapped files unavailable in the browser sandbox). Set it explicitly if you need to override it.

## Platform paths

Database paths are resolved automatically per platform and aren't configurable through `ConnectionOptions` — see [Features > Platform Handling](features/platform-handling.md) for what each platform resolves to.
