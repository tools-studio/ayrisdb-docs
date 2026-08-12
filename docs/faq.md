# FAQ

**Do I have to write raw SQL?**
No. The attribute-based ORM covers `CreateTableAsync`, `InsertAsync`, `UpdateAsync`, `DeleteAsync`, and `QueryAsync` without you writing SQL by hand. Raw SQL is also fully supported through `ExecuteAsync` and `QueryAsync` when you need it. See [ORM and CRUD](features/orm-and-crud.md).

**Which platforms does AyrisDB support?**
Native SQLite binaries are included for Windows (x64 and x86), Linux (x64), and WebGL. Opening a connection on a platform outside this list will fail — see [Troubleshooting](troubleshooting.md).

**Is my database encrypted by default?**
No. Encryption is opt-in — see [Encryption](features/encryption.md).

**Does AyrisDB work with IL2CPP?**
Yes, automatically. See [IL2CPP Safety](features/il2cpp-safety.md).

**Can I use LINQ `Where`-clause expressions in a query?**
No. LINQ `Where`-clause expressions require runtime code generation that IL2CPP doesn't support. Use parameterized SQL instead — for example, `QueryAsync<T>("WHERE level > ?", 5)`.

**Does the Inspector add anything to my shipped build?**
No. The Inspector is an Editor-only window for browsing a connection's tables, schema, and rows while you develop. It isn't included in a player build.

**What's the difference between AyrisDB Free and AyrisDB Pro?**
AyrisDB Free covers everything documented here: connections, the ORM, Unity type storage, encryption, platform handling, and the Inspector. AyrisDB Pro, sold separately on the Asset Store, adds schema migrations, query profiling, and production database tooling on top of that. Code written against AyrisDB Free continues to work unmodified if you upgrade.

**Can I use AyrisDB Free in a commercial game?**
Usage rights are governed by the Unity Asset Store's own license terms for this package, shown on the Asset Store listing at the time of purchase.

**Where do I report a bug or ask a question?**
See [Support](../SUPPORT).
