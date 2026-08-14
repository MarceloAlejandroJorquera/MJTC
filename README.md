# MJTC — MJ Torrent Client

<img width="1824" height="1023" alt="Screenshot_800" src="https://github.com/user-attachments/assets/62260826-6aac-418c-9825-2452974d1eca" />

**MJTC** is a high-performance BitTorrent client for Windows built around an **in-house torrent engine** and a dense, real-time monitoring interface.

Rather than hiding protocol activity behind a single progress bar, MJTC exposes torrent, peer, piece, transport, verification, DHT, GeoIP, and transfer state directly in the UI.

> **Performance · Visibility · Control**

[![Release](https://img.shields.io/badge/Release-v1-blue)](../../releases/latest) [![Status](https://img.shields.io/badge/Status-Stable-brightgreen)](../../releases/latest) ![Platform](https://img.shields.io/badge/Platform-Windows%20x64-0078D6) ![.NET](https://img.shields.io/badge/.NET-10-512BD4) ![Distribution](https://img.shields.io/badge/Distribution-Single--file%20binary-555555)

## Download

Download the latest stable binary from **Releases**:

`MJTC-v1-win-x64.exe`

MJTC v1 is published as a **self-contained, single-file Windows x64 executable**. A separate .NET runtime installation is not required.

Each stable release also includes `SHA256SUMS.txt` so the downloaded executable can be verified before running it.

```powershell
Get-FileHash .\MJTC-v1-win-x64.exe -Algorithm SHA256
```

Compare the result with the value in `SHA256SUMS.txt` from the same release.

---

## Highlights

- Custom BitTorrent engine developed specifically for MJTC
- `.torrent` and magnet-link support
- BitTorrent v1, v2, and hybrid torrent support
- TCP and µTP operating concurrently
- DHT, Peer Exchange, Local Peer Discovery, and HTTP/UDP trackers
- Web-seed engine support *(included in v1, not yet surfaced as a desktop feature)*
- Protocol Encryption / Message Stream Encryption support
- High-throughput piece scheduling and endgame handling
- Selective file downloading and file-level priorities
- Built-in torrent verification and force-check workflows
- Detailed torrent, peer, and file progress visualization
- Live piece maps and temporary purple new-progress overlays
- Rich peer telemetry with client/version identification
- Configurable Peer ID profiles using recognized client signatures
- Integrated GeoIP city/country lookup and vector country/subdivision flags
- Global and per-torrent bandwidth controls
- Persistent torrent/session state and historical transfer statistics
- Transfer graphs and configurable refresh cadence
- System-tray integration and fast restore
- Optimized single-file distribution with embedded runtime assets

---

# Getting Started

## System requirements

- **Operating system:** Windows x64
- **Runtime:** Included in the executable
- **Framework target:** .NET 10 / WPF
- **Network:** Internet access and permission through the local firewall

MJTC stores its application state under:

```text
%LOCALAPPDATA%\MJTC
```

The application organizes persistent data into subdirectories for state, network identity, statistics, logs, cache, lists, downloads, and other runtime data.

## Installation

MJTC is portable and does not require a traditional installer.

1. Download `MJTC-v1-win-x64.exe` from the latest GitHub release.
2. Optionally verify its SHA-256 checksum against `SHA256SUMS.txt`.
3. Place the executable wherever you want to keep it.
4. Start MJTC.
5. If Windows Firewall asks for network permission, allow the network access appropriate for your environment.

If Windows displays a reputation or publisher warning, review the executable and release checksum before proceeding. Windows warnings do not by themselves indicate that the downloaded file differs from the release asset.

---

# Adding Torrents

## Open a `.torrent` file

1. Open MJTC.
2. Use the torrent-add control or open a `.torrent` file associated with MJTC.
3. Choose the destination and file selection as required.
4. Start the torrent.

MJTC parses the torrent metadata, initializes the selected files, restores reusable local data when applicable, and begins peer discovery through the mechanisms permitted by that torrent.

## Add a magnet link

1. Copy a magnet URI.
2. Open **Add Magnet** in MJTC.
3. Paste the magnet and confirm.
4. MJTC begins peer discovery and metadata acquisition.

Magnet support includes metadata exchange and file-selection parameters where supplied by the URI.

---

# Torrent Management

MJTC supports the normal torrent lifecycle together with lower-level controls not normally exposed by simple downloaders.

### Common actions

- Start
- Pause
- Resume
- Remove torrent
- Remove torrent and optionally remove payload data
- Force-check / verify local data
- Select or deselect individual files
- Change file priority
- Apply torrent-specific bandwidth limits
- Apply torrent-specific connection limits
- Inspect torrent details and piece state
- Export/copy torrent and magnet information where available

Paused torrents are removed from active transfer scheduling rather than merely being visually marked as paused.

---

# Custom BitTorrent Engine

MJTC does not wrap a third-party torrent engine. The application contains its own BitTorrent implementation, including:

- Bencoding parsing and writing
- Torrent metainfo parsing
- Peer handshakes and peer-wire messages
- Choking / unchoking
- Piece and block requests
- Piece scheduling
- Piece verification
- Upload scheduling
- Endgame behavior
- Tracker communication
- DHT
- Peer Exchange
- Local Peer Discovery
- Web seeds *(engine implementation included; not yet surfaced in the v1 desktop workflow)*
- Magnet metadata acquisition
- TCP transport
- µTP transport
- Protocol encryption / obfuscation
- NAT traversal / hole punching
- Torrent persistence and resume state

Keeping the engine inside MJTC allows networking and scheduling behavior to be tuned together with the application's UI telemetry and persistence model.

---

# Peer Discovery

MJTC can discover peers through multiple independent sources:

- HTTP trackers
- HTTPS trackers
- UDP trackers
- DHT
- Peer Exchange (`ut_pex`)
- Local Peer Discovery when enabled
- Magnet-provided peers
- Hole-punch/rendezvous paths when negotiated and applicable

Private torrents are treated specially: discovery mechanisms that must not be used for private swarms are disabled for those torrents.

---

# TCP + µTP

MJTC supports **TCP and µTP concurrently**.

The transport controls are designed to influence the proportion of connections without unnecessarily starving one transport. The engine can replenish connections from trackers, DHT, and PEX as peer availability changes.

µTP implements the BitTorrent µTP transport, including SACK handling and congestion/retransmission behavior.

---

# Protocol Encryption / Obfuscation

MJTC supports BitTorrent **Message Stream Encryption / Protocol Encryption (MSE/PE)** for peer connections.

Modes can permit, require, or refuse encrypted peer handshakes depending on configuration.

MSE/PE should be understood as **protocol obfuscation and compatibility functionality**, not as end-to-end anonymity or a replacement for transport security/privacy tools.

---

# Piece Scheduling and Endgame

The transfer engine contains an availability-aware piece and block scheduler with support for:

- Per-peer request windows
- Adaptive request pipelines
- Request timeout recovery
- Pipeline refill
- Duplicate-request protection
- Unique-block-first behavior
- Stale-request recovery
- Selective-download awareness
- Endgame scheduling
- Bounded rescue duplicates
- Cancellation of unnecessary duplicate requests

Near completion, MJTC attempts to keep healthy peer pipelines productive without turning endgame into uncontrolled duplicate-request traffic.

---

# Torrent Verification and Data Integrity

MJTC verifies torrent payload against the hashes defined by the torrent format rather than assuming that bytes present on disk are valid.

Verification support includes:

- Piece hash verification
- File-aware force checking
- Per-piece verification state
- Verification progress visualization
- Recovery after files are deleted or externally modified
- Synchronization between verification and active downloading
- Resume-state integration
- v1 SHA-1 piece verification
- v2 Merkle/SHA-256 verification
- Merkle torrent verification where applicable

If previously downloaded files are manually removed, MJTC can re-evaluate local piece ownership and download the missing data again.

---

# Files and Selective Downloads

The **Files** view exposes per-file state inside each torrent.

Depending on the torrent format and state, MJTC tracks:

- File path
- File size
- Selection state
- Priority
- Completion
- Piece distribution
- Verification state
- Newly downloaded progress

Magnet file-index selection is supported when provided by the magnet URI.

---

# Progress Visualization

MJTC uses detailed progress visualization across the Torrents, Peers, and Files views.

The UI can represent:

- Overall completion
- Piece ownership
- Selected-file completion
- Missing pieces
- Active transfer state
- Availability
- Verification state
- Newly acquired data

### State colors

- **Green** — active/downloading state
- **Teal** — completed / fully available state
- **Purple** — newly received progress
- **Gray** — paused / inactive state
- **Red** — blocked or error-related state where applicable
- **Orange** — warning/intermediate state where applicable

New progress is temporarily highlighted with a **purple overlay** so recent acquisition can be distinguished from older completed data.

---

# Detailed Peer Monitoring

MJTC exposes live and historical information for peers, including:

- IP address
- Port
- Country flag
- Client identity
- Client version
- TCP / µTP transport
- Encryption state where available
- Download rate
- Upload rate
- Downloaded amount
- Uploaded amount
- Piece availability
- Completion percentage
- Connection state
- Blocked state
- First seen / last seen information
- Session information
- Geographic information

Disconnected, blocked, and previously observed peers can remain visible according to the application's history/retention behavior instead of disappearing immediately.

---

# Peer ID and Client Identification

MJTC implements BEP 20 peer-ID recognition and maintains a broad catalog of known client signatures.

Recognized families include, among others:

- qBittorrent
- µTorrent
- BitTorrent
- Transmission
- BiglyBT / Vuze / Azureus
- Tixati
- MonoTorrent
- libtorrent
- libTorrent (Rakshasa)
- BitWombat
- Folx
- Xunlei
- Free Download Manager
- WebTorrent
- LibreTorrent
- PicoTorrent
- Gopeed
- aria2
- many additional historical and current BitTorrent implementations

The **Peer ID & Port** control can generate a fresh 20-byte Peer ID using registered client profiles and their version/signature conventions. Unknown or opaque peer IDs are not force-mapped to a known client and may be displayed as randomized/unavailable instead.

---

# GeoIP and Flags

MJTC integrates geographic peer information directly into the peer interface.

Features include:

- Country detection
- City lookup when available
- Country/subdivision flag rendering
- Peer-detail geographic information
- Embedded GeoIP payload
- Indexed vector flag resource pack

The runtime does not require a loose GeoIP database or a directory full of SVG files beside the executable.

### GeoLite attribution

This product includes GeoLite Data created by MaxMind, available from [MaxMind](https://www.maxmind.com/).

---

# DHT

MJTC contains an integrated BitTorrent DHT implementation with IPv4/IPv6-aware behavior.

The engine includes support for routing-table maintenance, bootstrap, peer discovery, external-address evidence, DHT security extensions, read-only behavior, mutable/immutable items, and other DHT extensions listed in the BEP table below.

The UI exposes DHT state such as running status, buckets, and known nodes.

---

# Trackers

Tracker support includes:

- HTTP/HTTPS announce
- UDP announce
- Compact peer lists
- IPv4 and IPv6 peers
- Multi-tracker tiers
- Tracker scrape
- UDP tracker extensions
- DNS tracker preference handling
- Retry metadata where provided

### Web-seed engine support

<sub>MJTC v1 includes BEP 17/BEP 19 web-seed parsing and transfer code in the engine, but web seeds are **not yet surfaced as a normal desktop feature/control**. They are therefore treated as an engine-only capability in the BEP table below rather than an advertised v1 workflow.</sub>

---

# Bandwidth and Connection Controls

MJTC provides both global and per-torrent controls.

These include:

- Global download limit
- Global upload limit
- Per-torrent download limit
- Per-torrent upload limit
- Per-torrent connection limits
- TCP / µTP preference
- Connection replenishment behavior
- Encryption policy
- DHT/PEX/LSD-related controls
- Advanced networking limits

---

# Persistent State

MJTC keeps torrent/session state across application restarts.

Persisted data can include:

- Torrent metadata/repository data
- Piece completion
- File selection
- Running/paused state
- Resume data
- Peer-related history where applicable
- Verification state
- Network identity/port settings
- Historical statistics
- UI/window state

Performance-sensitive state uses compact binary formats where appropriate, with legacy migration paths retained where needed.

---

# Statistics and Graphs

MJTC maintains session and historical statistics, including:

- Torrents downloaded
- Data downloaded
- Data uploaded
- Sharing percentage
- DHT status
- DHT buckets
- DHT nodes

The **Sharing** value is expressed as a percentage and is not capped at 100%.

```text
Sharing = Uploaded / Downloaded × 100
```

MJTC also provides global and per-torrent transfer graphs. Per-torrent graph history can continue accumulating while a different torrent is selected.

---

# Streaming

The engine contains file-streaming support for consuming selected torrent files while data is being acquired, subject to piece availability and the torrent/file state.

Streaming-aware scheduling can prioritize the pieces needed by the active stream without disabling normal torrent integrity checks.

---

# System Tray and Desktop Integration

MJTC includes Windows desktop integration such as:

- System-tray operation
- Fast hide/restore behavior
- Window-state persistence
- `.torrent`/magnet startup argument handling
- Single-instance forwarding
- Custom multi-resolution application icon

---

# BitTorrent Enhancement Proposal (BEP) Support

MJTC v1 contains a broad set of BitTorrent protocol implementations. The table distinguishes between capabilities used by the current application and code that is present in the engine but is not yet surfaced by the v1 desktop workflow.

**Status legend**

- **Active** — used by normal MJTC v1 operation or directly exposed by the application.
- **Automatic** — used internally when the protocol/torrent requires it; no dedicated control is necessary.
- ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) — implementation is included in the binary but is not yet used/surfaced as a normal MJTC v1 desktop feature. These rows are intentionally visually muted.

| BEP | Feature | MJTC v1 scope | Status |
|---:|---|---|---|
| 3 | BitTorrent Protocol Specification | Core peer-wire protocol, metainfo, trackers, piece transfer | **Active** |
| 5 | DHT Protocol | DHT node, peer discovery, PORT messages | **Active** |
| 6 | Fast Extension | Have All/None, Reject, Suggest, Allowed Fast | **Automatic** |
| 7 | IPv6 Tracker Extension | IPv6 HTTP/UDP tracker announce and peer parsing | **Automatic** |
| 9 | Extension for Peers to Send Metadata Files | Magnet metadata acquisition (`ut_metadata`) | **Active** |
| 10 | Extension Protocol | Extended handshake and negotiated extension IDs | **Automatic** |
| 11 | Peer Exchange (PEX) | `ut_pex` receive/broadcast with private-torrent restrictions | **Active** |
| 12 | Multitracker Metadata Extension | Tracker tiers / multi-tracker operation | **Active** |
| 13 | Protocol Encryption | MSE/PE peer protocol obfuscation support | **Active** |
| 14 | Local Service Discovery | Local Peer Discovery, configurable | **Active** |
| 15 | UDP Tracker Protocol | UDP announce/scrape support | **Active** |
| 16 | Superseeding | Initial/super-seed mode and Fast-extension integration | **Active** |
| <sub>17</sub> | <sub>HTTP Seeding (Hoffman-style)</sub> | <sub>Web-seed parsing/transfer implementation is present, but web seeds are not yet surfaced as a normal v1 desktop workflow.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| <sub>19</sub> | <sub>HTTP/FTP Seeding (GetRight-style)</sub> | <sub>Web-seed parsing/transfer implementation is present, but web seeds are not yet surfaced as a normal v1 desktop workflow.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| 20 | Peer ID Conventions | Client decoding and configurable peer-ID profiles | **Active** |
| 21 | Extension for Partial Seeds | Upload-only/partial-seed signaling | **Automatic** |
| 23 | Tracker Returns Compact Peer Lists | Compact peer response parsing | **Automatic** |
| 24 | Tracker Returns External IP | External-address evidence used by tracker/network identity paths | **Automatic** |
| 27 | Private Torrents | Disables DHT/PEX/LSD/hole-punch paths as required | **Automatic** |
| 29 | µTorrent Transport Protocol | µTP transport, SACK and retransmission/congestion logic | **Active** |
| 30 | Merkle Hash Torrent Extension | Merkle torrent parsing, proof/hash-piece transport and verification | **Automatic** |
| 31 | Tracker Failure Retry Extension | Tracker retry directives and backoff handling | **Automatic** |
| 32 | IPv6 Extension for DHT | IPv6 DHT operation | **Automatic** |
| <sub>33</sub> | <sub>DHT Scrape</sub> | <sub>Scrape request/response and Bloom-filter code is present, but the current v1 application does not initiate the DHT scrape workflow.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| 34 | DNS Tracker Preferences | DNS tracker preference resolution | **Automatic** |
| 35 | Torrent Signing | Signature metadata parsing and verification | **Automatic** |
| <sub>36</sub> | <sub>Torrent RSS Feeds</sub> | <sub>Feed parser/service code is included, but the v1 desktop application does not expose feed subscriptions or auto-add controls.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| 38 | Finding Local Data Via Torrent File Hints | Authenticated local payload reuse/hint handling during add/restore | **Automatic** |
| 39 | Updating Torrents Via Feed URL | Update URL monitoring and successor-torrent handling | **Automatic** |
| 40 | Canonical Peer Priority | Canonical peer-priority calculation | **Automatic** |
| 41 | UDP Tracker Protocol Extensions | UDP tracker extension support | **Automatic** |
| 42 | DHT Security Extension | Secure DHT node-ID/address handling | **Automatic** |
| <sub>43</sub> | <sub>Read-only DHT Nodes</sub> | <sub>Read-only DHT mode exists in engine configuration, but it is not currently surfaced by the v1 desktop application.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| 44 | Storing Arbitrary Data in the DHT | Immutable/mutable DHT item support; also underpins mutable-torrent functionality | **Automatic** |
| 45 | Multiple-address Operation for the BitTorrent DHT | Multi-address / IPv4+IPv6 DHT behavior | **Automatic** |
| 46 | Updating Torrents Via DHT Mutable Items | Mutable-torrent magnet resolution, subscriptions and update handling | **Active** |
| 47 | Padding Files and Extended File Attributes | Padding, symlink/attribute metadata and file-layout handling | **Automatic** |
| 48 | Tracker Protocol Extension: Scrape | Extended tracker scrape handling | **Automatic** |
| <sub>49</sub> | <sub>Distributed Torrent Feeds</sub> | <sub>Metadata parsing, validation and builder/feed primitives are included, but distributed-feed operation is not yet surfaced in the v1 desktop workflow.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| <sub>50</sub> | <sub>Publish/Subscribe Protocol</sub> | <sub>DHT topic publish/subscribe service and protocol handling are included, but the v1 desktop application does not expose topic subscription/publishing.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| <sub>51</sub> | <sub>DHT Infohash Indexing</sub> | <sub>`sample_infohashes` responder/sampling infrastructure is included, but no normal v1 desktop workflow initiates or exposes infohash sampling.</sub> | ![Engine only](https://img.shields.io/badge/v1-engine%20only-6e7781) |
| 52 | BitTorrent Protocol Specification v2 | v2/hybrid metainfo, SHA-256 Merkle trees, hash exchange | **Active** |
| 53 | Magnet URI Extension — Select Specific File Indices | `so=` file-index/range selection | **Active** |
| 54 | `lt_donthave` Extension | Piece-withdrawal send/receive semantics | **Automatic** |
| 55 | Holepunch Extension | `ut_holepunch` rendezvous/connect/error handling | **Automatic** |

Official BitTorrent Enhancement Proposal index: [bittorrent.org/beps](https://www.bittorrent.org/beps/bep_0000.html)

---

# Security and Privacy

MJTC does not require an account or a hosted MJTC service to operate. Application state is stored locally on the Windows machine.

BitTorrent itself is a peer-to-peer protocol. When participating in a swarm, your network address can be visible to peers, trackers, DHT nodes, web seeds, and other infrastructure involved in that torrent. **MJTC is not an anonymity tool.**

Protocol Encryption / MSE can obfuscate BitTorrent traffic but does not make peer-to-peer activity anonymous.

Use MJTC only for data you are legally authorized to download, distribute, or share.

---

# Troubleshooting

## MJTC does not start

- Verify that you downloaded the executable from the repository's official Releases page.
- Verify the SHA-256 checksum.
- Check whether Windows Security or another endpoint-security product quarantined the file.
- Review `%LOCALAPPDATA%\MJTC\Logs\crash.log` if a crash log was generated.

## Downloads are slow

- Check the torrent's seed/peer availability.
- Check global and per-torrent bandwidth limits.
- Check the per-torrent connection limit.
- Verify that Windows Firewall permits MJTC's network traffic.
- Check tracker and DHT state.
- Review whether TCP/µTP preferences or encryption settings are overly restrictive.

## Magnet metadata does not arrive

- Confirm that the magnet has a valid info hash.
- Ensure DHT/trackers are available for that torrent.
- Wait for a peer supporting metadata exchange.
- Private torrents depend on their permitted tracker/peer-discovery paths.

## A completed torrent becomes incomplete

If payload files were removed or modified outside MJTC, run a force check. The client will re-evaluate the local data and reacquire missing pieces as needed.

## Firewall/NAT problems

Allow MJTC through Windows Firewall as appropriate. NAT traversal, UPnP/NAT-PMP, and peer hole punching can help where supported, but successful incoming connectivity still depends on the router, ISP, firewall, and remote peer.

---

# Support and Bug Reports

Use the repository's **Issues** section for reproducible defects and feature requests.

For bug reports, include as much of the following as possible:

1. MJTC version (for example `v1`)
2. Windows version/build
3. Whether the torrent was `.torrent` or magnet-based
4. Torrent type when known: v1, v2, hybrid, private, web-seeded, etc.
5. Exact steps to reproduce the issue
6. Expected behavior
7. Actual behavior
8. Relevant screenshots
9. Relevant MJTC settings
10. Relevant crash/log information from `%LOCALAPPDATA%\MJTC\Logs`

Do **not** post private tracker passkeys, authentication tokens, personal paths, or other secrets in public issues.

For performance reports, also include approximate torrent size, piece count, peer count, connection limit, TCP/µTP preference, and observed transfer rate.

---

# Release and Versioning Policy

Public MJTC versions use compact version labels without redundant zero components:

```text
v1
v1.1
v1.1.1
v1.1.2
v1.2
v2
```

Pre-release labels may use a descriptive suffix, for example:

```text
v1-beta
```

The release asset follows the same public version:

```text
MJTC-v1-win-x64.exe
MJTC-v1.1-win-x64.exe
MJTC-v1.1.1-win-x64.exe
```

---

# Distribution

MJTC is currently distributed as a **compiled Windows binary**. Application source code is not included with the binary release.

The executable is self-contained and carries the runtime resources needed by the application, including optimized embedded assets used by the UI, GeoIP, and flag system.

---

# Legal Notice

MJTC is provided for legitimate peer-to-peer file transfer. Users are responsible for ensuring that they have permission to download and distribute the content they use with the application.

Third-party data and technologies remain subject to their respective terms and licenses.

This product includes GeoLite Data created by MaxMind, available from [https://www.maxmind.com/](https://www.maxmind.com/).

---

# Project Direction

MJTC is designed as both a practical torrent client and an observable BitTorrent implementation.

Development priorities include:

> **Throughput · Protocol Correctness · Reliability · Persistence · UI Accuracy · Large-Swarm Performance · Transfer Integrity · Observability**
