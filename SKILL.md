# Name: LeviLamina C++ Modding Best Practices
# Description: Expert guidelines for writing native C++ mods for Minecraft Bedrock using LeviLamina, covering lifecycle hooks, event subscriptions, and manifest configuration.

## Core Guidelines
- **Target Standard:** Always use modern **C++20** features safely (smart pointers, RAII, and standard containers).
- **Mod Lifecycle:** Native mods interface through LeviLamina's `ll::mod::NativeMod`. Lifecycle functions typically include `load()` for setup, `enable()` when the mod activates, and `disable()` for cleanup.
- **Manifest Requirements:** Every mod must include a valid `manifest.json` defining its name, version, and type as a native mod.
- **Event Handling:** Use LeviLamina's event bus system to safely subscribe to game events (e.g., player joins, block interactions) without blocking the main server/client thread.
- **Build System:** Projects utilize **XMake** (`xmake.lua`) to compile dynamic libraries (`.so` for Android/Linux or `.dll` for Windows).

- ## Android NDK & Multi-Architecture Rules
- **Multi-ABI Targeting:** Always compile native libraries to support both `arm64-v8a` (modern 64-bit devices) and `armeabi-v7a` (older legacy devices) to prevent architecture mismatch crashes.
- **XMake Configuration:** When configuring builds via XMake for Android, explicitly target architectures using:
  `xmake f -p android -a arm64-v8a` (and build a separate `armeabi-v7a` target if needed).
- **No Platform-Specific Hacks:** Avoid hardcoded absolute file paths, Windows headers (`<windows.h>`), or private linker symbols that fail on Android's dynamic loader (`dlopen`).
- **Standard C++ STL:** Always link against the shared LLVM libc++ STL (`c++_shared`) to avoid symbol duplication crashes when multiple modules load.
- 
