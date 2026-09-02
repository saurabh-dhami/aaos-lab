# Chapter 9: The Android Boot Sequence – From Power Button to Operating System

## 1. The 4 Steps to Wake Up Android
When you turn on an Android device (or start our Cuttlefish emulator), the system goes through 4 strict stages to load the OS:

1. **Boot ROM** ➔ The absolute first spark. This is a tiny piece of permanent code burned into the processor chip at the factory. Its only job is to find the storage disk, load the next step into memory, and run it.
2. **Bootloader** ➔ The traffic cop. This program turns on your phone's main RAM memory, double-checks that the operating system hasn't been hacked (security verification), and unpacks the Linux Kernel into memory.
3. **Linux Kernel** ➔ The engine room. The kernel takes full control of the physical hardware. It sets up memory zones, loads hardware drivers (like screen or power chips), and finishes by starting the granddaddy process of Android called **`init`**.
4. **Native `init` Process (Process ID 1)** ➔ The master builder. This is the very first user-visible process to run. It reads setup files called `init.rc` to create standard system folders, set up folder permissions, and fire up background system workers.

## 2. Understanding the `init.rc` Script Files
The `init` process builds the Android runtime environment using instructions written in script files ending in `.rc` (Android Init Language). These scripts use 3 simple building blocks:

* **Actions** ➔ The "When" (Events). This tells the system *when* to do something (e.g., `on boot` or `on early-init`).
* **Commands** ➔ The "What" (Tasks). The actual instructions executed inside an action block (e.g., `mkdir /data/system` to create a folder).
* **Services** ➔ The "Workers" (Programs). Background programs that `init` must turn on and keep running (e.g., launching the system logger tool `logcatd`).
