# Godot Webview OSR

A lightweight, cross-platform **Off-Screen Rendering (OSR)** WebView extension for **Godot 4** via GDExtension.

Unlike traditional overlay webviews, `godot-webview-osr` captures rendered browser frames directly into Godot `ImageTexture` buffers in memory. This enables transparent 2D HUD interfaces, full UI integration, and projection onto in-game 3D computer monitors and interactive surfaces.

> ⚠️ **Note:** This project is currently under active development. APIs and features may change before a stable release.

---

## Features

- **True Off-Screen Rendering (OSR):** Frames are rendered directly to dynamic textures without overlay windows interfering with the engine.
- **2D & 3D Ready:**
  - `WebViewOSR`: A `TextureRect`-derived node featuring built-in input forwarding and auto-resizing.
  - `WebViewOSR3D`: A ready-to-use 3D node designed for in-game interactive screens, RayCast UV interaction, and emissive screen materials.
- **Native OS Backends:**
  - **Windows:** Microsoft Edge WebView2 (Chromium).
  - **Linux:** WebKitGTK / Cairo off-screen surface.
  - **Android:** Native Android WebView off-screen bitmap pipeline.
- **Bidirectional IPC:** Seamless message passing between GDScript and web pages using `window.godot.send()` and `emit_to_html()`.
- **Local Asset Resolution:** Built-in protocol support for `res://` and `user://` virtual directories.
- **Input Synthesis:** Forward mouse movements, clicks, scrolls, touch events, and keyboard strokes directly to the web canvas.

---

## Supported Platforms

| Platform | Engine Backend | Status |
| :--- | :--- | :--- |
| **Windows (x86_64)** | Microsoft Edge WebView2 | In Development |
| **Linux (x86_64)** | WebKitGTK 4.0 / 4.1 | In Development |
| **Android (ARM64)** | Android `android.webkit.WebView` | In Development |

---

## Architecture Overview

```text
Godot Scene Tree (WebViewOSR / WebViewOSR3D)
       │
       ▼
GDExtension (IWebViewBackend Interface)
       │
   ┌───┴───────────────────────┬────────────────────────┐
   ▼                           ▼                        ▼
Windows (WebView2)      Linux (WebKitGTK)       Android (WebView)
   │                           │                        │
   └───────────► RGBA8 Pixel Buffer ◄───────────────────┘
                       │
                       ▼
            Godot Image / ImageTexture
