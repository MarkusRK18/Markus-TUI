# Installation Guide

Comprehensive installation instructions for **Markus-TUI / MovieBox-TUI** across Linux, macOS, Windows, and Android (Termux).

---

## Table of Contents

- [System Prerequisites](#system-prerequisites)
  - [Media Players](#media-players)
  - [Terminal Poster Previews](#terminal-poster-previews)
  - [Optional Stream Downloaders](#optional-stream-downloaders)
- [Installation Methods](#installation-methods)
  - [Method 1: Build & Install from Source (Recommended)](#method-1-build--install-from-source-recommended)
  - [Method 2: Cargo Local Install](#method-2-cargo-local-install)
- [Distribution-Specific Dependencies](#distribution-specific-dependencies)
  - [Ubuntu / Debian / Pop!_OS / Linux Mint](#ubuntu--debian--pop_os--linux-mint)
  - [Arch Linux / Manjaro / EndeavourOS](#arch-linux--manjaro--endeavouros)
  - [Fedora / RHEL](#fedora--rhel)
  - [macOS](#macos)
  - [Android (Termux)](#android-termux)
  - [Windows](#windows)
- [Verifying Installation](#verifying-installation)
- [Configuration & Getting Started](#configuration--getting-started)
- [Uninstallation](#uninstallation)

---

## System Prerequisites

### Media Players
To stream movies, series, or live TV channels, install at least one supported player:

| Player | Platform | Recommended Flags / Notes |
| :--- | :--- | :--- |
| **mpv** *(Highly Recommended)* | Linux, macOS, Windows | Best hardware acceleration & subtitle sync. |
| **VLC** | Cross-platform | Broad format compatibility. |
| **IINA** | macOS | Modern GUI media player for macOS. |
| **Android Video Player** | Android / Termux | VLC, Just Player, MX Player. |

### Terminal Poster Previews
To display high-resolution movie posters and cover art directly in the terminal, use a terminal that supports image protocols (Kitty graphics protocol or Sixel):
- **Ghostty**
- **Kitty**
- **WezTerm**
- **iTerm2**

*(Standard terminals like Alacritty, Foot, or Windows Terminal automatically fall back to clean ASCII text placeholders).*

### Optional Stream Downloaders
- **`yt-dlp`** and **`ffmpeg`**: Only required if downloading protected or DASH streams from the MovieBox provider. All other providers stream and download directly with the built-in HTTP engine.

---

## Installation Methods

### Method 1: Build & Install from Source (Recommended)

1. **Install the Rust toolchain** (if not already installed):
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   source "$HOME/.cargo/env"
   ```

2. **Clone the repository**:
   ```bash
   git clone git@github.com:MarkusRK18/Markus-TUI.git
   cd Markus-TUI
   ```

3. **Compile the optimized release binary**:
   ```bash
   cargo build --release
   ```

4. **Install the executable to your `PATH`**:
   - **For your user only (Recommended)**:
     ```bash
     mkdir -p ~/.local/bin
     cp target/release/moviebox-tui ~/.local/bin/
     ```
     Ensure `~/.local/bin` is in your `$PATH` (e.g. in `~/.bashrc`, `~/.zshrc`, or `~/.config/fish/config.fish`):
     ```bash
     export PATH="$HOME/.local/bin:$PATH"
     ```

   - **System-wide (requires sudo)**:
     ```bash
     sudo install -Dm755 target/release/moviebox-tui /usr/local/bin/moviebox-tui
     ```

---

### Method 2: Cargo Local Install

You can build and install directly using Cargo into `~/.cargo/bin`:

```bash
git clone git@github.com:MarkusRK18/Markus-TUI.git
cd Markus-TUI
cargo install --path . --locked
```

Ensure `~/.cargo/bin` is present in your `$PATH`.

---

## Distribution-Specific Dependencies

### Ubuntu / Debian / Pop!_OS / Linux Mint
```bash
sudo apt update
sudo apt install -y build-essential pkg-config libssl-dev mpv curl git
```
*Optional for DASH downloads:*
```bash
sudo apt install -y ffmpeg yt-dlp
```

### Arch Linux / Manjaro / EndeavourOS
```bash
sudo pacman -Syu --needed base-devel openssl mpv curl git
```
*Optional for DASH downloads:*
```bash
sudo pacman -S --needed ffmpeg yt-dlp
```

### Fedora / RHEL
```bash
sudo dnf groupinstall "Development Tools"
sudo dnf install -y openssl-devel mpv curl git
```

### macOS
1. Install [Homebrew](https://brew.sh) (if not already installed).
2. Install dependencies:
   ```bash
   brew install rust mpv git
   ```
3. Clone and build:
   ```bash
   git clone git@github.com:MarkusRK18/Markus-TUI.git
   cd Markus-TUI
   cargo build --release
   cp target/release/moviebox-tui /usr/local/bin/ # or ~/.local/bin/
   ```

### Android (Termux)
1. Update packages and install dependencies:
   ```bash
   pkg update && pkg upgrade -y
   pkg install -y rust binutils make openssl curl git termux-tools termux-am
   ```
2. Setup storage permissions:
   ```bash
   termux-setup-storage
   ```
3. Clone and build:
   ```bash
   git clone git@github.com:MarkusRK18/Markus-TUI.git
   cd Markus-TUI
   cargo build --release
   cp target/release/moviebox-tui $PREFIX/bin/
   ```

### Windows
1. Install [Rust via rustup](https://rustup.rs/).
2. Install [mpv](https://mpv.io/) or [VLC](https://www.videolan.org/vlc/).
3. In PowerShell / Command Prompt:
   ```powershell
   git clone git@github.com:MarkusRK18/Markus-TUI.git
   cd Markus-TUI
   cargo build --release
   ```
4. Copy `target\release\moviebox-tui.exe` to a directory in your system `%PATH%`.

---

## Verifying Installation

Verify that the binary is accessible in your environment:

```bash
moviebox-tui --version
```
Expected output:
```text
moviebox-tui 0.1.20
```

To run the application:
```bash
moviebox-tui
```

---

## Configuration & Getting Started

### Configuration Location
Configuration and cache are stored at:
- **Linux**: `~/.config/moviebox-tui/config.json`
- **macOS**: `~/Library/Application Support/moviebox-tui/config.json`
- **Windows**: `%APPDATA%\moviebox-tui\config.json`

### Key Features & Controls
- **Search & Play**: Type any title on the home screen, select with <kbd>↑</kbd>/<kbd>↓</kbd>, press <kbd>Enter</kbd> to stream.
- **Batch Downloading**:
  - Press <kbd>m</kbd> on any movie in search results or favorites to mark/unmark it.
  - Press <kbd>d</kbd> to prompt batch download for all marked items.
  - Press <kbd>c</kbd> to clear all marks.
- **Concurrent Download Limit**:
  - Open preferences with `/settings` $\rightarrow$ General.
  - Cycle **Concurrent Downloads** (1 to 5, default: **3**).
- **Provider Switching**:
  - Press <kbd>Ctrl+P</kbd> to cycle between MovieBox, 4KHDHub, BDIX CircleFTP, BDIX DhakaFlix, or Addons.
- **Live TV Mode**:
  - Press <kbd>Ctrl+T</kbd> to switch to Live TV / IPTV mode.
  - Press <kbd>Ctrl+S</kbd> to switch back to Streaming mode.
- **Interactive Help**:
  - Press <kbd>?</kbd> anywhere in the TUI to open the full keybinding help menu.

---

## Uninstallation

To remove the binary:

```bash
# If installed in ~/.local/bin:
rm -f ~/.local/bin/moviebox-tui

# If installed via cargo:
cargo uninstall moviebox-tui

# To optionally delete user configuration, history, and cache:
rm -rf ~/.config/moviebox-tui ~/.cache/moviebox-tui
```
