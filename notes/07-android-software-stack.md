📱 Android Software Stack (5 Layers)
System Apps:  The top layer containing core apps (Launcher, Settings, Dialer) that provide the baseline UX using elevated system permissions.
Java API Framework: The developer's dashboard exposing Manager classes (e.g., LocationManager) that act as secure proxies to system services.
ART & Native Libraries: The engine room where the Android Runtime manages code compilation while raw C/C++ libraries (SQLite, OpenGL) handle heavy performance tasks.
HAL (Hardware Abstraction Layer): A universal adapter that defines standard API contracts so the OS can talk to proprietary, closed-source vendor hardware drivers.
Linux Kernel: The absolute concrete foundation that handles low-level resource management like CPU scheduling, memory management, and process security.


┌──────────────────────────────────────────────────────────────────┐
│ 1. SYSTEM APPS LAYER                                             │
│    (Launcher, Settings, Dialer -> Pre-installed Baseline UX)     │
└─────────────────────────────────┬────────────────────────────────┘
                                  │ Uses Java APIs
┌─────────────────────────────────▼────────────────────────────────┐
│ 2. JAVA API FRAMEWORK LAYER                                      │
│    (Managers, View System -> Secure Proxy Interface Dashboard)   │
└─────────────────────────────────┬────────────────────────────────┘
                                  │ Calls via JNI / Binder IPC
┌─────────────────────────────────▼────────────────────────────────┐
│ 3. ENGINE ROOM LAYER (ART & NATIVE C/C++ LIBRARIES)              │
│    (Android Runtime -> Ahead-of-Time Bytecode Compilation)      │
│    (Native Libraries -> SQLite, WebKit, OpenGL Engine)           │
└─────────────────────────────────┬────────────────────────────────┘
                                  │ Standardized API Contracts
┌─────────────────────────────────▼────────────────────────────────┐
│ 4. HAL LAYER (HARDWARE ABSTRACTION LAYER)                         │
│    (Universal Adapter -> Isolates Closed-Source Vendor Drivers)  │
└─────────────────────────────────┬────────────────────────────────┘
                                  │ Talks to Kernel Drivers
┌─────────────────────────────────▼────────────────────────────────┐
│ 5. FOUNDATION LAYER (LINUX KERNEL)                               │
│    (Low-Level OS -> CPU Scheduling, RAM Allocator, OOM Killer)   │
└─────────────────────────────────┬────────────────────────────────┘
                                  │ Drives Directly
┌─────────────────────────────────▼────────────────────────────────┐
│ PHYSICAL HARDWARE (Silicon, Camera Sensor, GPS, RAM Chips)       │
└──────────────────────────────────────────────────────────────────┘