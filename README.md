```text
 ___  _ _____ ___ _  _ ___ ___
| _ \/_\_   _/ __| || | __/ __|
|  _/ _ \| || (__| __ | _|\__ \
|_|/_/ \_\_| \___|_||_|___|___/ by thisangelo
```
---
## Index

* [Available patches and their reason](#available-patches-and-their-reason)
  * [Open A Terminal Here](#open-a-terminal-here)
  * [DC Offset Pop Audio](#dc-offset-pop-audio)
* [Operating system specifications](#operating-system-specifications)
---
## Available patches and their reason:

### **Open A Terminal Here**
Makes the `Open a terminal here` option of file managers work in the other  
terminals that are not native, in this case for Ptyxis (GNOME Terminal Emulator).

**Tested terminal:**

* [Ptyxis](https://gitlab.gnome.org/chergert/ptyxis)

**Tested file managers:**

* [Dolphin](https://apps.kde.org/en/dolphin/)

* [Nemo](https://github.com/linuxmint/nemo)

---

### **DC Offset Pop Audio**
It prevents that clicking sound or faint noise from coming through the speakers  
when the computer restarts—an annoying problem that only occurs when restarting  
under Linux—by having SystemD run this external patch to resolve the issue.

**Tested Linux Distribution:**

* [Fedora 44 KDE Plasma](https://fedoraproject.org/kde/)

**Tested Computer**

* [HP Laptop 14-ep0xxx](https://support.hp.com/us-en/product/troubleshooting/hp-14-inch-laptop-pc-14-ep0000/model/2102175168?sku=A47CQLA&serialnumber=1H944802WM)

---
### Operating system specifications

<details>
<summary>Click to see my system info</summary>

```text

             .',;::::;,'.                 thisangelo@fedora
         .';:cccccccccccc:;,.             -----------------
      .;cccccccccccccccccccccc;.          OS: Fedora Linux 44 (KDE Plasma Desktop Edition) x86_64
    .:cccccccccccccccccccccccccc:.        Host: HP Laptop 14-ep0xxx
  .;ccccccccccccc;.:dddl:.;ccccccc;.      Kernel: Linux 7.2.15-200.fc44.x86_64
 .:ccccccccccccc;OWMKOOXMWd;ccccccc:.     Uptime: 9 hours, 45 mins
.:ccccccccccccc;KMMc;cc;xMMc;ccccccc:.    Packages: 26 (flatpak-system), 13 (flatpak-user), 3628 (rpm)
,cccccccccccccc;MMM.;cc;;WW:;cccccccc,    Shell: bash 5.3.9
:cccccccccccccc;MMM.;cccccccccccccccc:    Display (AUO1999): 1366x768 in 14", 60 Hz [Built-in]
:ccccccc;oxOOOo;MMM000k.;cccccccccccc:    Desktop Environment: KDE Plasma 6.8.2
cccccc;0MMKxdd:;MMMkddc.;cccccccccccc;    Window Manager: KWin (Wayland)
ccccc;XMO';cccc;MMM.;cccccccccccccccc'    WM Theme: ChromeOS-dark
ccccc;MMo;ccccc;MMW.;ccccccccccccccc;     Theme: Breeze (Dark) [Qt], Breeze-Dark [GTK2], Breeze [GTK3/4]
ccccc;0MNc.ccc.xMMd;ccccccccccccccc;      Icons: breeze-dark [Qt], breeze-dark [GTK2/3/4]
cccccc;dNMWXXXWM0:;cccccccccccccc:,       Font: Adwaita Sans (11pt) [Qt], Adwaita Sans (11pt) [GTK2/3/4]
cccccccc;.:odl:.;cccccccccccccc:,.        Cursor: breeze (24px)
ccccccccccccccccccccccccccccc:'.          Terminal: Ptyxis 50.1
:ccccccccccccccccccccccc:;,..             Terminal Font: JetBrainsMonoNL Nerd Font (10pt, SemiBold, Italic)
 ':cccccccccccccccc::;,.                  CPU: 12th Gen Intel(R) Core(TM) i3-1215U (4+4) @ 4.40 GHz
                                          GPU: Intel UHD Graphics @ 1.10 GHz [Integrated]
                                          Memory: 7.72 GiB / 15.32 GiB (50%)
                                          Swap: 124.00 KiB / 8.00 GiB (0%)
                                          Disk (/): 42.02 GiB / 70.00 GiB (60%) - btrfs
                                          Disk (/home): 195.64 GiB / 328.57 GiB (60%) - ext4
                                          Battery (AE03041): 100% (3 hours, 53 mins remaining) [Discharging]
                                          Locale: es_ES.UTF-8

```

</details>