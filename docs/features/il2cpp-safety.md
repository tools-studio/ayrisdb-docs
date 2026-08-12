# IL2CPP Safety

## Purpose

IL2CPP strips managed code it can't prove is reachable by static analysis, which normally requires you to maintain a `link.xml` file by hand to preserve reflection-dependent types.

## Workflow

Before every IL2CPP build, the Editor generates `AyrisDB.Generated.link.xml` automatically, preserving every `[Table]`-decorated type from stripping. You don't create or edit this file yourself.

## Configuration

No configuration is required. The generated file is rebuilt on every build, so it stays in sync as you add or remove `[Table]` classes.

## Example

Build for IL2CPP as you normally would. If a query behaves correctly in the Editor but returns no results in an IL2CPP build, confirm you're using parameterized SQL rather than a LINQ `Where`-clause expression — see [Troubleshooting](../troubleshooting.md).
