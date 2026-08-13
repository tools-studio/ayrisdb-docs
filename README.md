# AyrisDB

![AyrisDB](docs/images/cover.png)

SQLite for Unity, with zero configuration.

AyrisDB Free — public Unity Asset Store release.

This repository is AyrisDB's documentation source — the pages themselves, not the product. If you're looking for the product, it's on the Unity Asset Store; if you're reading this on GitHub directly rather than a published site, everything below lives under `docs/`.

<p>
  <a href="docs/getting-started.md"><img src="https://img.shields.io/badge/Getting%20Started-lightgrey?style=for-the-badge" alt="Getting Started"></a>
  <a href="docs/installation.md"><img src="https://img.shields.io/badge/Installation-lightgrey?style=for-the-badge" alt="Installation"></a>
  <a href="docs/features/"><img src="https://img.shields.io/badge/Features-lightgrey?style=for-the-badge" alt="Features"></a>
  <a href="docs/api-reference.md"><img src="https://img.shields.io/badge/API-lightgrey?style=for-the-badge" alt="API Reference"></a>
  <a href="SUPPORT"><img src="https://img.shields.io/badge/Support-lightgrey?style=for-the-badge" alt="Support"></a>
</p>

<p>
  <a href="https://discord.gg/VrbxQ9vnrT"><img src="https://img.shields.io/badge/Discord-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Discord"></a>
  <a href="https://discord.gg/XPMGcdnpmn"><img src="https://img.shields.io/badge/Report%20Issue-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Report Issue"></a>
  <a href="https://discord.gg/Ge99xqt5qr"><img src="https://img.shields.io/badge/Feature%20Request-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Feature Request"></a>
</p>

## What AyrisDB does

AyrisDB gives Unity projects a SQLite database with an attribute-based ORM, automatic Unity type storage, optional AES-256 encryption, and automatic IL2CPP safety, without writing platform-specific code.

## Main features

- Zero-configuration SQLite connections, with platform paths and journal modes resolved automatically
- An attribute-based ORM with async and sync CRUD methods
- Automatic storage of Unity value types (Vector3, Quaternion, Color, Rect, Bounds) with no manual serialization code
- Optional AES-256 database encryption, with a guard that verifies encryption is actually active
- Automatic IL2CPP safety — `[Table]` types are preserved from code stripping before every build
- An in-Editor Inspector for browsing any open AyrisDB connection's tables, schema, and rows

## Documentation

- [Overview](docs/overview.md)
- [Getting Started](docs/getting-started.md)
- [Installation](docs/installation.md)
- [Configuration](docs/configuration.md)
- [Features](docs/features/)
- [Troubleshooting](docs/troubleshooting.md)
- [FAQ](docs/faq.md)
- [API Reference](docs/api-reference.md)

## Current release

**1.0.0.** Full release history is tracked in AyrisDB's own repositories rather than duplicated here.
