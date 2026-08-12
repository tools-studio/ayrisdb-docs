# Unity Type Storage

## Purpose

Storing a `Vector3`, `Quaternion`, `Color`, `Rect`, or `Bounds` normally means writing manual conversion code to and from a storable format. This feature removes that step.

## Workflow

Use any of these types directly as a property on a `[Table]` class. AyrisDB stores each type's components in split columns automatically and reconstructs the value when you read it back.

## Configuration

No configuration is required. To store a type this list doesn't cover, implement `ITypeConverter<T>` and register it with `AyrisDB.TypeConverters.Register<T>(new YourConverter())` before your first `OpenAsync` call.

## Example

```csharp
[Table("spawn_points")]
public class SpawnPoint
{
    [PrimaryKey, AutoIncrement] public int Id { get; set; }
    public Vector3 Position { get; set; }
}
```
