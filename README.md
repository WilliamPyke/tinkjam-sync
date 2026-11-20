# Tinkjam Sync

Tinkjam Sync connects a computer to [Tinkjam](https://tinkjam.com), so the AI agents installed on
it (Claude Code, Codex and others) can work on that person's boards. It runs in the menu bar or
system tray. Nobody needs a terminal or Node.js to use it.

This repository holds the three desktop apps:

| Folder | System | Built with |
|---|---|---|
| [`mac/`](mac) | macOS 13 or newer | Swift, AppKit, SwiftPM |
| [`windows/`](windows) | Windows 10 and 11, x64 | C#, WinForms, .NET Framework |
| [`linux/`](linux) | x86_64 desktops | Rust, egui |

Downloads are on [tinkjam.com/download](https://tinkjam.com/download).

## What each app does

1. Downloads a pinned Node.js into `~/.tinkjam/runtime` and checks its SHA-256.
2. Downloads the Sync package, checks its SHA-256 and its release signature, and installs it into
   `~/.tinkjam/app`.
3. Runs the package as a child process, shows its state, and stops it on Quit.
4. Opens the browser so the person can approve the computer on their Tinkjam account. Nothing
   connects until they press Approve there.
5. Checks for a newer app and a newer package, and installs only releases signed by the Tinkjam
   release key.

The Sync package itself (the part that talks to the agents and to Tinkjam) is built from the
[Tinkjam repository](https://github.com/WilliamPyke/tinkjam), which is also open source:
`scripts/build-sync-package.ts` there bundles `apps/server`.

## Release signatures

Every update manifest (`sync.json`, `mac.json`, `windows.json`, `linux.json`) carries an ECDSA P-256
signature over its fields. The apps hold the public keys and refuse an unsigned or wrongly signed
manifest, so a server alone cannot push an update. The code is in `ReleaseSignature.swift`,
`ReleaseSignature.cs` and `signature.rs`. Maintainers sign with `scripts/sign-release.ts` in the
Tinkjam repository; the build notes here assume it is checked out beside this one as `../tinkjam`.

## Building

- Mac: [`mac/README.md`](mac/README.md)
- Linux: [`linux/README.md`](linux/README.md)
- Windows: run `windows/build.ps1` in PowerShell. It needs the .NET SDK.
  `-CompileOnly` skips the installer.

A build you make yourself is not signed by the Tinkjam release key. It runs, and it still accepts
only updates that are.

## Licence

[MIT](LICENSE). The bundled Outfit and Instrument Sans fonts are under the SIL Open Font License;
their licence texts sit beside them. The agent logos belong to their owners.
