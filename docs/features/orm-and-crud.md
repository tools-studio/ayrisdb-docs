# ORM and CRUD

## Purpose

Lets you work with SQLite tables as plain C# classes instead of writing SQL by hand for common operations.

## Workflow

Decorate a class with `[Table]`, mark its primary key with `[PrimaryKey]`, and add any column-level attributes you need. Call `CreateTableAsync<T>()` once, then `InsertAsync`, `UpdateAsync`, `DeleteAsync`, and `QueryAsync` as needed. Every async method has a synchronous variant with the same name, minus the `Async` suffix.

## Configuration

`[Column(name)]` overrides a property's column name. `[Ignore]` excludes a property from all SQL operations. `[Unique]`, `[Indexed]`, and `[NotNull]` add their matching SQL constraints. `[AutoIncrement]` maps to `INTEGER PRIMARY KEY AUTOINCREMENT`.

## Example

```csharp
[Table("players")]
public class Player
{
    [PrimaryKey, AutoIncrement] public int Id   { get; set; }
    [NotNull]                   public string Name { get; set; }
}

await conn.CreateTableAsync<Player>();
await conn.InsertAsync(new Player { Name = "Alice" });
var players = await conn.QueryAsync<Player>("WHERE Name = ?", "Alice");
```

Raw SQL is also fully supported: `ExecuteAsync(sql, args)` for statements, `QueryAsync<T>(sql, args)` with a full `SELECT` for anything the attribute-based ORM doesn't cover. Use `?` placeholders for parameters rather than string concatenation. Avoid LINQ `Where`-clause expressions — they require runtime code generation that IL2CPP does not support. See the [API Reference](../api-reference.md) for the full method list.
