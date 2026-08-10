## In my own words

Lesson 3 traces four stages of the Android runtime, each fixing
the previous one's cost.

**1. Dalvik (JIT)**
Register-based VM instead of JVM's stack-based design: fewer
instructions for basic ops, less memory. `.class` to `.dex`.
Bytecode translated to native code live, every time the app
runs. Problem: JIT never stops, burns CPU and battery constantly.

**2. Pure AOT — ART (Android 5.0)**
`dex2oat` compiles the entire app to native code at install
time. Runtime CPU cost drops to zero, battery problem solved.
New problem: install time exploded (5 sec to 5 min for large
apps), OS updates forced recompiling every app on the device
(the 30-min "Android is upgrading" screen), and storage use
grew a lot.

**3. Hybrid / Profile-Guided JIT (Android 7.0)**
Install with zero AOT. First run uses JIT like old Dalvik,
while ART silently profiles which methods are "hot." When idle
and charging, `dexopt` AOT-compiles only the hot paths. Day 1
can stutter slightly; day 2 onward is native-speed on the paths
actually used, with no wasted storage compiling unused code.

**4. Cloud Profiles (Android 9.0)**
Reasoning: most apps' hot paths (login screen etc.) are near
universal across users, so why make everyone profile locally?
Early adopters' local profiles are anonymously aggregated by
Google Play into a master hot-path map, bundled with the APK
for new users. `dexopt` uses that map to AOT-compile the
critical paths before the app is even opened once, so day-1
users get day-5 performance. Compilation still happens 100%
on-device; the cloud only ships the list of what to compile,
not compiled code itself.

**Chain**: Dalvik (JIT always) to ART (AOT everything, at
install) to Hybrid (AOT only hot paths, when idle) to Cloud
assisted Hybrid (hot paths known in advance).

## Verified on device

- `adb shell cmd package compile -m speed -f com.android.chrome`
  forced full AOT compile, returned Success
- `adb shell dumpsys package dexopt | grep -A15 com.android.chrome`
  confirms `[status=speed] [reason=cmdline]`, matching the
  forced compile
- Comparing against an app never manually compiled (e.g. Gmail)
  shows `speed-profile` or `verify` instead: the actual
  compile-filter states from the lesson, visible per-app on a
  real device, not just Chrome