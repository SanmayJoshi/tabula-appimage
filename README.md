# Tabula Linux AppImage

<!--
[![Build and Release Tabula AppImage](https://github.com/SanmayJoshi/tabula-appimage/actions/workflows/build-appimage.yml/badge.svg)](https://github.com/SanmayJoshi/tabula-appimage/actions/workflows/build-appimage.yml)
[![GitHub Release](https://img.shields.io/github/v/release/SanmayJoshi/tabula-appimage?include_prereleases&style=flat-square)](https://github.com/SanmayJoshi/tabula-appimage/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](https://opensource.org/licenses/MIT)
-->

[Tabula](https://tabula.technology/) is an open-source tool for extracting data tables from PDF files into CSV format or Excel spreadsheets.

This repository provides an automated, standalone Linux AppImage for Tabula. It bundles everything needed to run the application in a single file, eliminating the need to install or configure Java on your system.

---

## Key Improvements

Running the original Tabula release on modern Linux distributions can be difficult due to older Java dependencies and browser integration issues. This package resolves those problems:

* **Includes Java**: Bundles an Eclipse Temurin OpenJDK 8 runtime. You do not need to install Java on your host system.
* **Updated Extraction Engine**: Upgraded the internal backend from the older Tabula release to `tabula-java` 1.0.5 and added LocationTech `jts-core` 1.20.0 for more accurate table detection.
* **Automatic Port Detection**: If default port `8080` is already in use by another service, Tabula automatically finds and binds to the next free port (such as `8081` or `8082`).
* **Reliable Browser Opening**: Replaces outdated Java browser calls with standard `xdg-open` handling, ensuring the interface opens correctly in your default web browser across modern desktop environments (including Wayland sessions).
* **Clear Control and Clean Shutdown**: Double-clicking the file in a file manager (like Dolphin or Nautilus) opens a terminal window showing the server status. You can stop the server at any time simply by pressing `Ctrl+C` or closing the terminal window.
* **Small Delta Updates**: Supports `zsync`, allowing you to download only the changed parts of the file when updating via AppImageUpdate.

---

## Requirements

* **Architecture**: 64-bit Linux (`x86_64`)
* **Operating System**: Any standard Linux distribution (Ubuntu, Fedora, Arch, Debian, openSUSE, etc.)

---

## Download and Usage

### Quick Start

1. Download the latest `Tabula-x86_64.AppImage` from the [Releases](https://github.com/SanmayJoshi/tabula-appimage/releases/latest) page.
2. Make the file executable:
   ```bash
   chmod +x Tabula-x86_64.AppImage
   ```
3. Run the application:
   * **From the file manager**: Double-click the AppImage file.
   * **From the terminal**:
     ```bash
     ./Tabula-x86_64.AppImage
     ```

A terminal window will open to display startup logs, and your default web browser will automatically open to `http://127.0.0.1:8080/` (or the next available port).

### Stopping Tabula

To stop the background server:
* Press `Ctrl+C` inside the terminal window, or
* Close the terminal window.

---

## Configuration

You can customize how Tabula runs using command-line flags or environment variables.

### Command-Line Flags

| Flag | Description |
| :--- | :--- |
| `-n`, `--no-browser` | Starts the server without automatically opening your web browser. |
| `--help` | Displays general Tabula help options. |

### Environment Variables

| Variable | Default | Description |
| :--- | :--- | :--- |
| `TABULA_PORT` | `8080` | The initial TCP port to try. If busy, the next port is used. |
| `TABULA_DATA_PATH` | `~/.local/share/tabula` | Directory where uploaded PDFs and data are saved. |
| `TABULA_JAVA_OPTS` | _(none)_ | Extra options passed to the Java runtime (for example: `-Xmx2048M`). |

#### Example: Running on a Custom Port Without Opening the Browser
```bash
TABULA_PORT=9000 ./Tabula-x86_64.AppImage --no-browser
```

---

## Desktop Integration

If you want Tabula to appear in your desktop application menu with an icon, you can use:

* **AppImage Managers**: Tools like [Gear Lever](https://github.com/mijorus/gearlever) or [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) will handle integration automatically.
* **Manual Setup**:
  ```bash
  # Extract the desktop file and icon
  ./Tabula-x86_64.AppImage --appimage-extract "tabula.desktop"
  ./Tabula-x86_64.AppImage --appimage-extract "tabula.png"

  # Copy files to your user application directories
  install -Dm644 squashfs-root/tabula.desktop ~/.local/share/applications/tabula.desktop
  install -Dm644 squashfs-root/tabula.png ~/.local/share/icons/hicolor/256x256/apps/tabula.png
  ```

---

## How It Is Built

The AppImage is built automatically using GitHub Actions:

1. Checks the upstream Tabula repository for new release tags.
2. Downloads and stages Eclipse Temurin OpenJDK 8 JRE.
3. Downloads the official `tabula.jar`, replaces the extraction backend with `tabula-java` 1.0.5, and adds `jts-core-1.20.0`.
4. Bundles necessary shared libraries and patches library search paths (`RPATH`).
5. Packages everything into an AppImage file using `appimagetool` and publishes the release assets.

---

## LLM Disclaimer

The CI/CD workflow automation, packaging recipes, and repository documentation were drafted and maintained with the help of Large Language Models (LLMs).

- **Core Application:** The underlying application source code is authored and maintained by upstream contributors.
- **As-Is Basis:** While the pipeline is reviewed to follow AppImage and Linux packaging best practices, the scripts and configurations are provided "as is", without warranty of any kind.

---

## Credits and License

* **Tabula**: Developed by [Manuel Aristarán](https://github.com/jazzido), [Mike Tigas](https://github.com/mtigas), and [Jeremy B. Merrill](https://github.com/jeremybmerrill).
* **Backend**: Powered by [tabula-java](https://github.com/tabulapdf/tabula-java).
* **Java Runtime**: Eclipse Temurin OpenJDK builds provided by the [Adoptium Working Group](https://adoptium.net/).

This packaging project is licensed under the [MIT License](LICENSE). Tabula is open-source software licensed under the MIT License.
