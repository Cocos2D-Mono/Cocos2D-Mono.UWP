# Changelog — Cocos2D-Mono.Uwp

All notable changes to the `Cocos2D-Mono.Uwp` and `Cocos2D-Mono.Box2D.Uwp` packages will be documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project adheres to a four-segment version line continued from the NuGet history (the latest pre-repo-split version was [2.4.8.3](https://www.nuget.org/packages/Cocos2D-Mono.Uwp/2.4.8.3) — Jan 14, 2025).

## [Unreleased]

### Added

- New repository home: [`Cocos2D-Mono/Cocos2D-Mono.UWP`](https://github.com/Cocos2D-Mono/Cocos2D-Mono.UWP). Source previously lived in `Cocos2D-Mono/cocos2d-mono` under `cocos2d/cocos2d.Uwp/` and `box2d/box2d.Uwp/`; those folders were removed from mainline in [`ea6a2d76`](https://github.com/Cocos2D-Mono/cocos2d-mono/commit/ea6a2d76) on 2024-12-01.
- `cocos2d-mono` source is now consumed via a git submodule pinned to commit `c30f5ec3` (parent of the removal commit). This is the last commit in mainline with the full `#if NETFX_CORE` source set intact.
- New companion package `Cocos2D-Mono.Box2D.Uwp` 2.4.8.4. Previously the Box2D code shipped inside `Cocos2D-Mono.Uwp` as a project reference; this release splits it for cleaner consumer dependency graphs.

### Changed

- Bumped to version `2.4.8.4` (continuation of the pre-split line).
- `<projectUrl>` and `<repository>` in nuspecs point to `Cocos2D-Mono/Cocos2D-Mono.UWP`.

### Pinned

| Dependency | Version |
|---|---|
| `MonoGame.Framework.WindowsUniversal` | 3.8.1.303 |
| `Microsoft.NETCore.UniversalWindowsPlatform` | 6.2.11 |
| `SharpDX.Mathematics` | 4.0.1 |
| `SharpZipLib` | 1.3.3 |
| `cocos2d-mono` source (submodule) | `c30f5ec3` |

## [2.4.8.3] — 2025-01-14

(Last version published from mainline `Cocos2D-Mono/cocos2d-mono` before the UWP folders were removed.)

## [2.4.8.2] — 2024-11-25
## [2.4.8.1] — 2024-10-23
## [2.4.8] — 2024-10-17

(Historical versions; refer to the [NuGet listing](https://www.nuget.org/packages/Cocos2D-Mono.Uwp/) for the full pre-repo-split version history.)
