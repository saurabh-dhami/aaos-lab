## Lesson 4 additions (Treble deep dive)

- **VINTF**: boot-time compatibility gate. Vendor declares
  supported HAL versions in /vendor/etc/vintf/manifest.xml;
  framework declares required versions in a compatibility
  matrix under /system/etc/vintf/. Init compares them at boot.
  Mismatch means the device refuses to boot entirely. This is
  what allows my phone's Android 16 framework to run against
  VNDK 34 vendor code.
- **SELinux enforces the partition boundary**, it is not just
  convention. A framework process physically cannot open a
  vendor hardware file. Visible via `ls -Z` on
  /vendor/lib64/hw vs /system/framework.
- **The real historical bottleneck was silicon vendors, not
  OEMs.** Qualcomm/MediaTek had to rewrite proprietary drivers
  per Android version per chip. Treble removed that bottleneck;
  the OEM bottleneck (/system is still Samsung's) is what
  Mainline addresses next.
- **GSI**: pure unmodified AOSP framework flashed onto /system
  to prove Treble compliance. Vendor partition and VINTF stay
  untouched, so a compliant device boots. Not a custom ROM.
  Google uses this via VTS to certify devices before they can
  ship with Play Services.