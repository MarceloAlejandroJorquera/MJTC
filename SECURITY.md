# Security Policy

## Supported Versions

Security fixes are provided for the current stable MJTC release.

| Version | Supported |
|---|---|
| `v1` | Yes |
| `v1-beta` | No |

Older releases and pre-release builds may remain available for archival purposes but are not actively supported.

## Reporting a Vulnerability

Please **do not open a public GitHub Issue** for a vulnerability that could put users at risk.

If private vulnerability reporting is available for this repository, use GitHub's private security reporting feature from the repository's **Security** section.

When reporting a vulnerability, include as much of the following as possible:

- A clear description of the issue
- The affected MJTC version
- Windows version and architecture
- Steps required to reproduce the issue
- Whether the issue can be triggered remotely
- The relevant protocol or input source, if known
- A minimal `.torrent`, magnet URI, tracker response, peer message, DHT message, or other test case when safe to provide
- Relevant MJTC log excerpts
- Screenshots or crash information when useful
- The security impact you believe is possible

Do not include copyrighted payload data, credentials, private keys, or unrelated personal information in a report.

If logs contain public IP addresses, peer identifiers, filesystem paths, or other potentially sensitive information, remove anything that is not necessary to reproduce the problem.

## Security-Relevant Areas

Examples of issues that should be reported privately include:

- Remote code execution
- Arbitrary file creation or overwrite
- Path traversal or unsafe torrent file paths
- Memory-safety or denial-of-service conditions caused by untrusted network input
- Malformed `.torrent` or magnet data causing unsafe behavior
- Unsafe handling of peer-wire, tracker, DHT, PEX, µTP, or extension-protocol messages
- Authentication, signature, or integrity-check bypasses
- Torrent verification failures that could accept corrupted data as valid
- Unsafe persistence or resume-state parsing
- Local privilege escalation
- Sensitive-data disclosure
- Security boundary bypasses
- Vulnerabilities in bundled or embedded dependencies that materially affect MJTC

Ordinary crashes, performance problems, incorrect UI behavior, compatibility issues, and feature requests should normally be filed as regular GitHub Issues unless they also have a security impact.

## Coordinated Disclosure

Please allow reasonable time for investigation and remediation before publicly disclosing a security issue.

A report may require additional information or a reproducible test case before the issue can be confirmed. Once a vulnerability is understood, fixes will be prioritized according to severity, exploitability, and user impact.

Security fixes may be released as a new stable version using MJTC's compact versioning scheme, for example:

- `v1`
- `v1.1`
- `v1.1.1`
- `v1.1.2`

Published release binaries include a SHA-256 checksum file so users can verify the exact downloaded executable.

## Binary Distribution

MJTC public releases are distributed as self-contained Windows x64 binaries.

The public repository may not contain the complete application source code. This does not limit security reporting: vulnerabilities in the released binary, supported protocols, embedded resources, persistence formats, or network behavior are still in scope.

## Network and Privacy Notes

MJTC is a BitTorrent client and therefore communicates with third-party peers, trackers, DHT nodes, and related BitTorrent services.

Protocol encryption or obfuscation features do **not** provide anonymity. Users should assume that BitTorrent participants may observe network addresses and protocol activity as part of normal operation.

## Legal and Responsible Testing

Only test systems, networks, torrents, trackers, peers, and data that you own or are authorized to test.

Do not use vulnerability research to disrupt other users, damage data, interfere with public BitTorrent infrastructure, or access systems without permission.
