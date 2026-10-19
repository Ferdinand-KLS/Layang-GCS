# Layang GCS (showcase)

A Windows ground control station for ArduPilot drones (copter, plane, rover, sub), built around the mistakes that most often cause crashes: values written to the vehicle must be proven correct, failsafes must be understandable, and dangerous actions must not fire from a mis-click.

> **This is a showcase repository.** The source code is private. Recruiters and engineers can request access via [LinkedIn](https://id.linkedin.com/in/ferdinand-klasse).

## Status
MVP 0.1, work in progress. Tested in software with a built-in simulator and an automated suite of **59 tests** (33 unit, 26 integration). Hardware testing with a real flight controller is the next step.

## What it does
- **Verified parameter writes.** One transaction: diff, validate, write, read back, report per parameter. Big integers and bitmasks go through MAVLink FTP so nothing is rounded silently.
- **Unit guard.** Warns about mistakes such as entering 45 centidegrees when 4500 was meant.
- **Mission upload with read-back and fingerprint.** Detects interrupted uploads and fake ACKs. Local plans are never overwritten.
- **Safety page.** Explains each failsafe as "what happens if...", and flags dangerous settings. PreArm messages are translated into plain language.
- **Hold-to-confirm** for disarm, force disarm and manual mode in flight.
- **Connection diagnosis.** Drivers, charge-only cables, baud rate, port in use, signing, packet loss.
- **MAVLink 2 signing**, keys stored with Windows DPAPI. No open network port by default.
- **Always-on telemetry log (tlog)**, crash-tolerant. Built-in practice simulator, including RC-loss simulation.
- **Ctrl+K command palette** for every page, action and parameter.

## Tech
Rust core (MAVLink 2 codec checked byte-for-byte against pymavlink, serial/UDP/TCP links, FTP, mission and parameter logic), Tauri 2 desktop shell, Svelte 5 + TypeScript UI, Leaflet map. About 12,000 lines of Rust.

## How it was built
I defined the problem from ArduPilot forum threads and GitHub issues, set the requirements, architecture and safety rules, and built it with AI-assisted coding, then verified the behavior with tests.

## License
Copyright (c) 2026 Ferdinand Klasse. All rights reserved.

No permission is granted to copy, modify, distribute, or use this material or the related source code without written permission from the author.
