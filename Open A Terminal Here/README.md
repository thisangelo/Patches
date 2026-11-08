# Open Terminal Here (Ptyxis Integration)

A lightweight Bash script designed to integrate **Ptyxis** as the default terminal emulator in Linux file managers such as **Dolphin (KDE Plasma 6)** and **Nemo (Cinnamon)**, allowing you to open the terminal directly in the current directory.

---

## Problem
In desktop environments like KDE Plasma 6 or Cinnamon, setting custom terminal emulators (like **Ptyxis**) to open directly from file managers often fails to pass the current working directory (`$PWD`) correctly, or fails entirely because native (RPM/DEB) and Flatpak packages expect different arguments.

> **Note:** The issue is not in Ptyxis itself, but in how file managers launch custom terminals and pass the working directory. This script acts as an intermediary with a fixed name and builds the correct command for you.

---

## Solution
A clean Bash wrapper script installed system-wide (`/usr/local/bin`) that captures the `$PWD` environment variable passed by the file manager and properly passes it to **Ptyxis**, ensuring new windows or tabs open seamlessly in the active directory.

> **Tested on:** Fedora Linux (KDE Plasma 6). Other distributions and Cinnamon/Nemo should work but are untested.

---

## Features

- Opens **Ptyxis** automatically pointing to the `$PWD` path invoked by the file manager.
- Compatible with native Ptyxis packages (RPM/DEB) as well as Flatpak installations.
- Support for **KDE Plasma 6** and **Cinnamon (Nemo)**.
- Easy installation into `/usr/local/bin`.

---

## Installation

1. Detect the installation method.

To check how Ptyxis is installed, run:

```bash
rpm -q ptyxis 2>/dev/null && echo RPM
dpkg -s ptyxis 2>/dev/null && echo DEB
flatpak list 2>/dev/null | grep -i ptyxis && echo Flatpak
```

2. Create the script depending on the installation method of Ptyxis.

- RPM/DEB

```bash
sudo tee /usr/local/bin/Open_a_terminal_here << 'EOF'
#!/bin/bash

exec ptyxis --new-window --working-directory="$PWD"
EOF
```

- Flatpak

```bash
sudo tee /usr/local/bin/Open_a_terminal_here << 'EOF'
#!/bin/bash

exec flatpak run org.gnome.Ptyxis --new-window --working-directory="$PWD"
EOF
```

*(Or use `--tab` instead of `--new-window` if you prefer to open tabs inside an existing window).*

> **Flatpak note:** If Ptyxis is installed as a Flatpak, the sandbox may block access to paths outside `$HOME` (e.g. `/mnt/...`), and the terminal would open in `~` instead. To check the current permissions, run `flatpak info --show-permissions org.gnome.Ptyxis`.

3. Make the script executable:

```bash
sudo chmod 755 /usr/local/bin/Open_a_terminal_here
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

## Author

Created and maintained by [@thisangelo](https://github.com/thisangelo)