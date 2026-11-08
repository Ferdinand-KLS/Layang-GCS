# Layang GCS (showcase)

A Windows ground control station for ArduPilot drones (copter, plane, rover, sub), built around the mistakes that most often cause crashes: values written to the vehicle must be proven correct, failsafes must be understandable, and dangerous actions must not fire from a mis-click.

> **This is a showcase repository.** The source code is private. Recruiters and engineers can request access via [LinkedIn](https://id.linkedin.com/in/ferdinand-klasse).

## Status
Work in progress, version 0.x. About 42,000 lines of Rust and 6,000 lines of Svelte/TypeScript.
- **165 automated tests pass** (137 unit + 27 integration in the core, 1 in the dev bridge; 5 manual tests are skipped). Last full run: 8 Nov 2026.
- Tested against the **official ArduPilot SITL** (ArduCopter 4.7.2): connect, GUIDED, arm, takeoff to 10 m and land.
- Windows installers (NSIS, MSI) and a portable build are produced and the `.exe` was checked on my laptop.
- **Not yet tested:** a real flight controller, a clean-PC installer run, and Windows 10.

## What it does
- **Verified parameter writes.** One transaction: diff, validate, write, read back, report per parameter. Big integers and bitmasks go through MAVLink FTP so nothing is rounded silently. A unit guard catches mistakes such as 45 centidegrees vs 4500.
- **Mission upload with read-back and fingerprint.** Detects interrupted uploads and fake ACKs, and never overwrites local plans. Smooth map at 60 fps with a 1,000-waypoint mission.
- **Safety page and plain-language PreArm explanations.** Hold-to-confirm for disarm and dangerous actions.
- **Connection doctor.** Wrong serial protocol or baud, charge-only USB cable, port in use, signing, packet loss.
- **DroneCAN.** Node list, node info, parameters, save, restart and firmware update, through the flight controller or an SLCAN adapter. The codec matches the official `pydronecan` library byte for byte (19 reference transfers).
- **Official SITL launcher.** Downloads the official ArduPilot SITL build only after the user agrees, checks file integrity before every start, and supports X-Plane and FlightGear options.
- **Firmware migration helper.** Maps renamed parameters between firmware versions using official metadata.
- **Photo geotagging from flight logs.** Writes GPS (EXIF) and Pix4D XMP into copies, never the originals.
- **Offline maps (MBTiles)**, **English and Indonesian UI**, window-state memory, MAVLink 2 signing with Windows DPAPI key storage, no open network port by default, always-on telemetry log, Ctrl+K command palette, built-in practice simulator.

## Tech
Rust core (MAVLink 2 codec checked byte for byte against pymavlink, serial/UDP/TCP/SLCAN links, FTP, DroneCAN, mission and parameter logic), Tauri 2 desktop shell, Svelte 5 + TypeScript UI, Leaflet map, SQLite (read-only) for MBTiles.

## How it was built
I defined the problem from ArduPilot forum threads and GitHub issues, set the requirements, architecture and safety rules, and built it with AI-assisted coding. Every feature is checked with tests, and `STATUS.md` in the private repo records what is tested and what is not.

## License
Copyright (c) 2026 Ferdinand Klasse. All rights reserved.

No permission is granted to copy, modify, distribute, or use this material or the related source code without written permission from the author.
