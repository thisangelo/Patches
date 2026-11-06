# Open Terminal Here (Ptyxis Integration)

A lightweight Bash script designed to integrate **Ptyxis** as the default terminal emulator in Linux file managers such as **Dolphin (KDE Plasma 6)** and **Nemo (Cinnamon)**, allowing you to open the terminal directly in the current directory.

---

## Problem
In desktop environments like KDE Plasma 6 or Cinnamon, setting custom terminal emulators (like **Ptyxis**) to open directly from file managers often fails to pass the current working directory (`$PWD`) correctly, or fails entirely when handling native (RPM/DEB) vs. Flatpak package arguments.

> **Compatibility Note:** This patch targets file manager terminal invocation behavior. Arguments and path variables depend on your desktop environment settings and Ptyxis installation type.

---

## Solution
A clean Bash wrapper script installed system-wide (`/usr/local/bin`) that catches the `$PWD` environment variable passed by the file manager and properly passes it to **Ptyxis**, ensuring new windows or tabs open seamlessly in the active directory.

---

## Features

- Opens **Ptyxis** automatically pointing to the `$PWD` path invoked by the file manager.
- Compatible with native Ptyxis packages (RPM/DEB) as well as Flatpak installations.
- Support for **KDE Plasma 6** and **Cinnamon (Nemo)**.
- Easy installation into `/usr/local/bin`.

---

## Installation

1. Clone or download this repository.
2. Grant execution permissions and install the script to the system path:

```bash
sudo install -m 755 ./Open_a_terminal_here /usr/local/bin/Open_a_terminal_here
```

---

## Configuration by Desktop Environment

### Dolphin (KDE Plasma 6)

Run the following commands in your terminal to set the script as the default terminal application and restart Dolphin:

```bash
kwriteconfig6 --file kdeglobals --group General --key TerminalApplication /usr/local/bin/Open_a_terminal_here
kwriteconfig6 --file kdeglobals --group General --key TerminalService --delete
pkill dolphin
```

> **Note:** You can open Dolphin again and press `F4` or use the menu option to verify that Ptyxis opens in the current directory.

---

### Nemo (Cinnamon)

Run the following commands in your terminal to register the executable and restart Nemo:

```bash
gsettings set org.cinnamon.desktop.default-applications.terminal exec '/usr/local/bin/Open_a_terminal_here'
gsettings set org.cinnamon.desktop.default-applications.terminal exec-arg ''
nemo -q
```

---

## Flatpak Support

If you use **Ptyxis** as a Flatpak package (system or user installation), edit the third execution line in `/usr/local/bin/Open_a_terminal_here`:

**Replace:**
```bash
exec ptyxis --new-window --working-directory="$PWD"
```

**With:**
```bash
flatpak run org.gnome.Ptyxis --new-window --working-directory="$PWD"
```

*(Or use `--tab` instead of `--new-window` if you prefer to open tabs inside an existing window).*

---

## Additional Ptyxis Options

- `--new-window`: Opens a standalone terminal window in the current directory.
- `--tab`: Opens a tab inside an already active Ptyxis window.

---

## Author

Created and maintained by [@thisangelo](https://github.com/thisangelo).