# Changelog

All notable public MJTC releases are documented here.

## v1 — 2026-08-13

First stable public release of the current MJTC architecture.

### Engine and networking

- Replaced the original lightweight beta architecture with MJTC's first-party BitTorrent engine.
- Added concurrent TCP and µTP transport support.
- Added configurable TCP/µTP preference without intentionally disabling either transport.
- Added tracker, DHT, and PEX peer discovery and peer replenishment.
- Added configurable per-torrent connection limits.
- Added global and per-torrent upload/download limiters.
- Added availability-aware piece scheduling, request-window management, timeout recovery, and adaptive endgame behavior.
- Added duplicate-request protection and bounded rescue requests near completion.
- Added selective-download-aware scheduling.
- Added blocked-peer exclusion from usable connection capacity.

### Integrity and verification

- Added piece-hash verification and force-check support.
- Added file-aware verification state and visual checking progress.
- Improved recovery after payload files are removed or become inconsistent.
- Tied completion state to verified torrent data rather than byte counters alone.

### Persistence

- Added compact binary engine resume state.
- Added per-torrent binary sidecars for piece/file state.
- Added legacy state migration.
- Added persistent running/paused state, file selection, verification state, and transfer statistics.
- Organized application state under `%LOCALAPPDATA%\MJTC\`.
- Significantly improved startup and shutdown behavior for larger sessions.

### Peers and client identification

- Added detailed peer history and telemetry.
- Added raw and decoded peer ID display.
- Added authoritative client-name normalization across known peer-ID formats.
- Added broad Azureus-style, Shadow-style, Mainline-style, and special-format client decoding.
- Added a Peer ID identity selector with randomized valid peer IDs and known client versions.
- Added table-formatted Client / ID / Version cascade presentation.
- Added TCP/µTP peer transport indication.
- Added blocked/disconnected peer retention and improved live peer refresh.

### GeoIP and flags

- Added compact embedded GeoIP city/country data.
- Added embedded indexed country/subdivision flag pack.
- Added vector flag rendering from memory with native aspect ratios.
- Added city/country peer details and flag/location tooltips.

### Torrent, peer, and file visualization

- Added piece-level progress and availability maps.
- Added dual progress visualization for torrent, peer, and file rows.
- Added live purple newly-downloaded progress overlays.
- Added state-aware green, teal, purple, gray, red, and warning presentation.
- Added combined percentage/progress presentation.
- Added verification-state visualization.
- Corrected stale self-peer completion coloring when local payload data is deleted and downloaded again.

### Interface

- Added custom dark WPF interface and custom title bar.
- Added `v1` title-bar badge.
- Added Torrents, Peers, Files, Details, graph, Statistics, Controls, Advanced, Lists, and DHT surfaces.
- Added configurable table refresh cadence.
- Added advanced search and filtering behavior.
- Added system tray integration and restored window-state handling.
- Added peer details modals, location information, copy/close glyph controls, and consistent modal styling.
- Improved large-swarm UI responsiveness with bounded/coalesced peer-history maintenance.

### Statistics and graphs

- Added global and per-torrent transfer graphs.
- Added persistent program statistics.
- Added Sharing percentage in place of a capped/decimal ratio presentation.
- Added sharing-state feedback bands.

### Packaging

- Updated to .NET 10 / C# 14.
- Published as a Windows x64 self-contained single-file executable.
- Embedded runtime fonts, GeoIP payload, optimized flag pack, and application assets.
- Added compact single-file bundle publishing.
- Added local-cache-first NuGet restore with online fallback for missing runtime/package assets.
- Added separate NuGet vulnerability auditing.

## v1-beta

Archived initial public beta release.

- Torrent file and magnet link support.
- Basic download management.
- Configurable application settings.
- Portable Windows executable.

The beta release is retained for historical purposes and has been superseded by `v1`.
