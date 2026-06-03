# Cocos2D-Mono.UWP

UWP / Xbox UWP support for [cocos2d-mono](https://github.com/Cocos2D-Mono/cocos2d-mono), maintained as a separate repository so the mainline can modernize without dragging UWP-era APIs along.

[![NuGet](https://img.shields.io/nuget/v/Cocos2D-Mono.Uwp.svg)](https://www.nuget.org/packages/Cocos2D-Mono.Uwp/)

## Status

This repo is the **source of truth for `Cocos2D-Mono.Uwp` on NuGet** (continuing the version line from [2.4.8.3](https://www.nuget.org/packages/Cocos2D-Mono.Uwp/2.4.8.3) released January 14, 2025). Mainline cocos2d-mono dropped its `cocos2d.Uwp` csproj on 2024-12-01 (commit [`ea6a2d76`](https://github.com/Cocos2D-Mono/cocos2d-mono/commit/ea6a2d76)) and the upcoming Phase 2 of mainline modernization will delete the `#if NETFX_CORE` source blocks entirely. This repo pins to mainline at commit `c30f5ec3` (the last commit with full UWP source intact) and stays viable independently.

## Pinned versions

| Dependency | Version | Rationale |
|---|---|---|
| `MonoGame.Framework.WindowsUniversal` | `3.8.1.303` | Last MonoGame UWP package published to nuget.org. (Mainline nuspecs reference `3.8.2.1105` but that version was never published.) |
| `Microsoft.NETCore.UniversalWindowsPlatform` | `6.2.11` | Last stable UWP runtime. |
| `SharpDX.Mathematics` | `4.0.1` | UWP-compatible. |
| `SharpZipLib` | `1.3.3` | Last UWP-compatible version. |
| mainline cocos2d-mono (submodule) | commit `c30f5ec3` | Last commit before the `cocos2d.Uwp` csproj was removed; full `#if NETFX_CORE` source intact. |

## Building

UWP class libraries require Visual Studio 2022 (any edition) with the **Universal Windows Platform development** workload installed.

```powershell
# Clone with the cocos2d-mono submodule
git clone --recurse-submodules https://github.com/Cocos2D-Mono/Cocos2D-Mono.UWP.git
cd Cocos2D-Mono.UWP

# If you already cloned without --recurse-submodules:
git submodule update --init --recursive

# Build (matching the legacy convention)
msbuild Cocos2D-Mono.Uwp.sln /p:Configuration=Release /p:Platform=x64 /restore
```

`dotnet build` cannot build UWP class libraries — use `msbuild` from a Developer Command Prompt.

## Packing for NuGet

```powershell
# After a successful Release build:
nuget pack nuget\Cocos2D-Mono.Uwp.nuspec       -OutputDirectory artifacts
nuget pack nuget\Cocos2D-Mono.Box2D.Uwp.nuspec -OutputDirectory artifacts
```

## Repository layout

```
Cocos2D-Mono.UWP/
├── Directory.Build.props               (pinned dependency versions)
├── Cocos2D-Mono.Uwp.sln                (Cocos2D.Uwp + Box2D.Uwp)
├── external/
│   └── cocos2d-mono/                   (git submodule, pinned to c30f5ec3)
├── src/
│   ├── Cocos2D.Uwp/
│   │   ├── Cocos2D.Uwp.csproj          (TargetPlatformIdentifier=UAP)
│   │   └── Properties/AssemblyInfo.cs
│   └── Box2D.Uwp/
│       ├── Box2D.Uwp.csproj            (TargetPlatformIdentifier=UAP)
│       └── Properties/AssemblyInfo.cs
├── nuget/
│   ├── Cocos2D-Mono.Uwp.nuspec
│   └── Cocos2D-Mono.Box2D.Uwp.nuspec
└── .github/workflows/build.yml         (windows-2022, msbuild)
```

> **NuGet package IDs vs repository name:** the repository is named `Cocos2D-Mono.UWP` (all-caps UWP) to match the project family casing on GitHub, but the published NuGet packages remain `Cocos2D-Mono.Uwp` and `Cocos2D-Mono.Box2D.Uwp` (mixed-case `Uwp`) to preserve continuity with the existing NuGet version line.

The csprojs use pre-SDK MSBuild format because UWP class libraries require `TargetPlatformIdentifier=UAP`, which the modern SDK-style csproj format does not support cleanly.

## Relationship to mainline cocos2d-mono

| Concern | This repo | Mainline cocos2d-mono |
|---|---|---|
| Target frameworks | `uap10.0` | `net9.0` / `net9.0-windows7.0` / `net9.0-android35.0` / `net9.0-ios18.0` (`net10.0*` on `release/2.6.0-preview`) |
| MonoGame | 3.8.1.303 (WindowsUniversal) | 3.8.4.1 (DesktopGL/WindowsDX/Android/iOS), 3.8.5-preview on preview |
| `#if NETFX_CORE` blocks in shared source | Retained (consumed from pinned submodule) | Slated for deletion in Phase 2 |
| Active modernization | None — frozen-archive maintenance | Phases 2–6 of [MODERNIZATION.md](https://github.com/Cocos2D-Mono/cocos2d-mono/blob/dev/MODERNIZATION.md) |

To bump the mainline source pin (e.g., to incorporate a security fix), update the submodule:

```powershell
cd external/cocos2d-mono
git fetch
git checkout <new-commit-hash>
cd ../..
git add external/cocos2d-mono
git commit -m "Bump mainline submodule to <new-commit-hash>"
```

Don't advance past Phase 1c (when mainline deletes the `#if NETFX_CORE` blocks). After that point, this repo's pin is frozen unless you cherry-pick fixes onto a long-lived branch in your own fork.

## License

This repository is licensed under [MIT](LICENSE), matching what `Cocos2D-Mono.Uwp` has historically declared on NuGet via the `<PackageLicenseExpression>MIT</PackageLicenseExpression>` field in every published `.nuspec`.

> **Note on the discrepancy with mainline:** mainline `cocos2d-mono`'s top-level `LICENSE` file is AGPL-3.0 while its csproj/nuspec declarations are MIT — a pre-existing inconsistency in the mainline repo. This UWP repo standardizes on MIT to match the package metadata its NuGet consumers have always seen. If mainline later resolves to a different license, revisit this file.
