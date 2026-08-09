## In my own words

Lesson 2 explains how Treble actually enforces the split
covered in Lesson 1 — with a real example: Samsung as OEM,
Qualcomm as silicon vendor.

**Legacy HAL problem**
Framework and vendor drivers lived in the same process,
linked directly via function pointers into a C struct. If
Google shifted a struct field or vtable slot, the layout the
vendor's compiled binary expected no longer matched — silent
data corruption, wrong vtable slot, or a missing symbol crash
at boot. This wasn't just framework → vendor; the vendor's
HAL also called back into AOSP libraries (libutils, libc++),
so a change there could break vendor code too — dependency
ran both directions.

**HAL as a contract**
The interface (`.aidl`/`.hal` file) is defined by Google and
lives in AOSP. The implementation is vendor code. Build tools
generate two separate classes from one interface file — a
proxy for the framework process and a stub for the vendor
process. Neither shares memory layout with the other; only
serialized bytes cross the boundary. That's why ABI drift no
longer breaks anything.

**Treble = two structural fixes**
1. HwBinder/HIDL (later Stable AIDL) — decouples framework
   calls to the HAL (fixes the top-down dependency)
2. VNDK — frozen per-version snapshot of AOSP libraries that
   vendor binaries link against, so Google can keep changing
   its own libraries without breaking vendor code that depends
   on them (fixes the bottom-up dependency)

**Camera streaming — control vs data plane**
Binder is not fast enough to carry 60fps raw frame data, so
it isn't used for that. Binder carries only small control
messages ("buffer ready", "start streaming"). The actual pixel
data moves through a shared memory buffer (gralloc) that the
sensor writes into directly; Binder just hands over a buffer
handle (fd) so both processes can access the same memory —
zero-copy. Same pattern Binder always plays: messenger, not
mover.

## Verified on device

Real device (Samsung), not emulator:

- `ro.treble.enabled` → true
- `ro.build.version.release` → 16 (framework)
- `ro.vndk.version` → 34 (vendor frozen snapshot, ~2 years
  older) — framework and vendor are two version generations
  apart and the phone still boots
- `df -h` shows /system, /system_ext, /vendor, /vendor_dlkm,
  /vendor/firmware_mnt as separate block devices — more than
  the two partitions the lesson describes
- `ps -A | grep camera` → cameraserver (framework, PPID 1561
  via zygote) vs android.hardware.camera.provider and
  vendor.samsung.hardware.camera.provider (vendor HAL, PPID 1
  via init) — three separate processes, separate UIDs
- `service list | grep camera` → android.hardware.ICameraService
  (framework), media.camera.proxy (a second proxy layer, app-facing),
  com.samsung.android.camera.ICameraServiceWorker (Samsung's own
  extension), vendor.qti.hardware.camera.offlinecamera (Qualcomm's
  own extension) — OEM and silicon vendor both add interfaces
  beyond the AOSP standard
- ICameraProvider registered separately as /external/0 and
  /internal/0 — front/back camera vs any board-mounted camera
- `ls /apex` → includes com.samsung.android.* modules, not just
  com.google.android.* — Mainline mechanism used by OEMs too,
  not just Google