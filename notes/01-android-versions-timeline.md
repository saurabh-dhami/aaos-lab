# 01 — Android Versions Timeline

Source: aospinsider.com — AOSP Foundations

## Core idea
Android's evolution is a story of solving bottlenecks.
Each architectural change came from the previous design's limit.

## Bottleneck to fix
| Problem | Solution | Version |
|---|---|---|
| CPU architecture unknown at build time | Dalvik + JIT | 1.0 |
| JIT burns CPU at launch, drains battery | ART + AOT (dex2oat) | 5.0 |
| OEM driver rewrites blocked every update | Treble — VINTF boundary | 8.0 |
| Security patches stuck behind OEM OTA | Mainline — APEX modules | 10 |

## In my own words
Android's architecture evolved in four steps, each fixing the
previous design's bottleneck.

**1. Hardware chaos (2007) → Dalvik + JIT**
Nobody knew whether a phone would run ARM, x86, or MIPS.
Compiling C/C++ directly meant a separate app per chip.
Fix: compile Java to `.dex` bytecode; the device translates it
to native code at app launch (JIT). This freed the app layer
from the hardware. HAL and kernel stayed native and
chip-specific — they always were.

**2. Performance wall (2013) → ART + AOT**
Translating bytecode on every launch burned CPU: jank and
battery drain. Fix: ART replaced Dalvik. `dex2oat` compiles
`.dex` into native machine code once, at install time.
Note the direction — build tools still produce `.dex`. AOT
consumes it, it doesn't produce it.

**3. Update crisis (Android 8) → Project Treble**
Framework and vendor drivers were entangled, so every OS
update forced OEMs to rewrite camera/GPU/audio drivers.
Months of work; most phones never got updated. Fix: VINTF —
a versioned interface wall between framework and HAL. An OEM
can now move the framework to Android 9 while reusing the
same Android 8 driver binaries.

**4. Patch crisis (Android 10) → Project Mainline**
Security fixes still needed an OEM OTA to reach users. Fix:
carve framework components into APEX packages shipped
directly via Play Store ("Google Play System Updates").

**Treble vs Mainline**
Treble is about the driver layer — it makes OEM updates
cheap, but OEMs still ship them. Mainline is about the
framework layer — Google ships those modules directly.
Mainline never touches drivers; that stays vendor territory.
Treble built the wall; Mainline made everything on Google's
side of it independently updatable.

## Verified on device
TODO
