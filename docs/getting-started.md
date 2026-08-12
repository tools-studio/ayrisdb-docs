# Getting Started

This walks through opening a database, creating a table, and writing and reading a row — the one path that gets you from a fresh install to a working database. For install steps themselves, see [Installation](installation.md) first if you haven't imported AyrisDB yet.

## Describe your data

Start by describing what you want to store as a plain C# class, with attributes marking the primary key and any columns that can't be null:

```csharp
using ToolsStudio.AyrisDB;

[Table("players")]
public class Player
{
    [PrimaryKey, AutoIncrement] public int Id    { get; set; }
    [NotNull]                   public string Name  { get; set; }
                                 public int Level { get; set; }
}
```

## Open a connection

AyrisDB resolves the correct database path and journal mode for whichever platform you're running on:

```csharp
var result = await AyrisDB.OpenAsync("game.db");
if (!result.IsSuccess)
{
    Debug.LogError(result.Error.Message);
    return;
}

using var conn = result.Value;
```

## Create the table and write a row

```csharp
await conn.CreateTableAsync<Player>();
await conn.InsertAsync(new Player { Name = "Alice", Level = 1 });
```

## Read it back

```csharp
var players = await conn.QueryAsync<Player>("WHERE level > ?", 0);
// players.Value is a List<Player>
```

That's a complete, working database. There's no `link.xml` to write by hand — your `[Table]` types are preserved from IL2CPP code stripping automatically.

## From here

- To see this running without writing any code yourself, use **Tools > AyrisDB > Create Demo Scene**, then press Play once.
- To browse a connection's tables, schema, and rows while your game is running, open **Tools > AyrisDB > Inspector**.
- For everything else AyrisDB can do, see [Features](features/).
