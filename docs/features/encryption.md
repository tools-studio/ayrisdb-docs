# Encryption

## Purpose

Protects a database's contents at rest with AES-256 encryption.

## Workflow

Derive a key from a passphrase with `AyrisDB.EncryptionKey.Derive(passphrase)`, then pass it as `ConnectionOptions.EncryptionKey` when opening the connection. A silent-failure guard checks that encryption is actually active after the connection opens, rather than failing silently if it isn't.

## Configuration

`EncryptionKey` defaults to `null` (no encryption). Once a database is created with a key, the same key must be supplied on every subsequent open.

## Example

```csharp
var options = new ConnectionOptions
{
    EncryptionKey = AyrisDB.EncryptionKey.Derive("my-passphrase")
};
var result = await AyrisDB.OpenAsync("game.db", options);
```

If a key is provided but the guard detects encryption isn't actually active, `OpenAsync` fails with `EncryptionNotActive` — see [Troubleshooting](../troubleshooting.md).
