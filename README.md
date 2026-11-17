# fxLauncher

The fx-CG50 launcher and its ESP32-S3 storage interface.

One launcher add-in is the only program installed on the calculator. It owns
the link to an ESP32-S3 (on the 2.5 mm serial port) whose microSD card holds
applications and their data. It preloads compressed data into a RAM page
cache, and loads applications from the SD card into RAM and runs them, so the
calculator's internal storage holds little more than the launcher itself.

| Part | Where | State |
|---|---|---|
| Link library: framing, page-pak reader, deflate decoder, page cache, page streaming, preload simulator | [`lib/cglink`](lib/cglink/README.md) | host-tested |
| Launcher add-in (gint): link client, page cache, preloader, application loader, SD file browser | `launcher/` | planned |
| ESP32-S3 firmware (ESP-IDF): link service, page server, FAT on SD, saves, audio, Wi-Fi file transfer | `firmware/` | planned |

The three share one protocol, so they are versioned together. The first
application is Project Picori (The Minish Cap), whose port plan and progress
are in HelaFaye/tmc, `fx-cg50` branch: `docs/fx-cg50-port-plan.md` and
`docs/fx-cg50-progress.md`.

## Hardware

An ESP32-S3FH4R2 SuperMini (4 MB flash, 2 MB PSRAM) with a microSD breakout.
There is no level shifter: measure the calculator's TX level before wiring
(see the tmc plan, "Wiring without a level shifter").

## Tests

```sh
cmake -S lib/cglink -B build/cglink && cmake --build build/cglink
ctest --test-dir build/cglink --output-on-failure
```
