## 🧩 Core Architecture Framework

### 1. The Partition Boundary
Project Treble decouples the monolithic Android OS by creating a absolute separation of concerns across physical storage allocations:
* **`/system`:** Houses Google’s generic, upstream AOSP framework and system APIs. Updatable independently.
* **`/vendor`:** Houses the silicon vendor's (Qualcomm/MediaTek) closed-source HAL implementations and device-specific kernel modules. Remains static during OS upgrades.

### 2. The Communication Contract: HIDL & AIDL
Because processes cannot cross the partition or talk directly due to SELinux, they use a highly strictly versioned IPC mechanism:
* **HIDL (Hardware Interface Definition Language):** The original legacy interface compiler introduced in Android 8 to auto-generate binderized code between framework and vendor.
* **Stable AIDL:** Modern Android (Android 11+) has largely deprecated HIDL in favor of Stable AIDL for system-to-vendor communication, enforcing backward compatibility natively.

### 🚗 Summary Analogy
* **Framework (`/system`)** is the driver.
* **Hardware Drivers (`/vendor`)** is the car engine.
* **HIDL/AIDL** is the steering wheel and pedals. You can change the driver completely, but as long as they press the same pedals, the engine reacts exactly the same way.
