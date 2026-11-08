# Intel SOF Reboot Audio Pop Fix (DC Offset Pop)

A lightweight systemd shutdown hook that eliminates electrical pop sounds (*DC Offset Pop*) on Intel laptops during system reboot.

---

## Problem

On Intel-based laptops using the open-source `sof-audio-pci-intel-tgl` audio driver, an audible electrical click or pop sound (*DC Offset Pop*) occurs on the left speaker when the system is rebooted.

My hypothesis is that this is caused by the kernel's audio driver performing an abrupt PCI bus reset during a reboot, without cleanly powering down the audio amplifier.

> **Compatibility Note:** This is an external, hardware-dependent patch. Its necessity and effectiveness vary based on your specific laptop model and audio circuit design.

---

## Solution

A lightweight systemd shutdown script that cleanly unbinds the audio controller from the PCI bus (`0000:00:1f.3`) strictly when the system performs a reboot operation.

> **Tested on:** Fedora Linux (KDE Plasma 6) on an HP laptop with an Intel Alder Lake audio controller.

---

## Features

- Unbinds the `sof-audio-pci-intel-tgl` driver before system reset.
- Triggered exclusively on `reboot` events (does not interfere with standard poweroff).
- Zero dependencies, running natively via systemd shutdown hooks.

---

## Installation

1. Detect your audio driver:

```bash
lspci -k | grep -A 3 -i audio
```

Example output (from my laptop):
```text
00:1f.3 Multimedia audio controller: Intel Corporation Alder Lake PCH-P High Definition Audio Controller (rev 01)
	Subsystem: Hewlett-Packard Company Device 8d27
	Kernel driver in use: sof-audio-pci-intel-tgl
	Kernel modules: snd_soc_avs, snd_sof_pci_intel_tgl, snd_hda_intel
00:1f.4 SMBus: Intel Corporation Alder Lake PCH-P SMBus Host Controller (rev 01)
	Subsystem: Hewlett-Packard Company Device 8b29
```

2. Create the shutdown script:

```bash
sudo tee /usr/lib/systemd/system-shutdown/fix-sof-reboot.shutdown << 'EOF'
#!/bin/sh

if [ "$1" = "reboot" ]; then
    if [ -d "/sys/bus/pci/drivers/sof-audio-pci-intel-tgl" ]; then
        echo 0000:00:1f.3 > /sys/bus/pci/drivers/sof-audio-pci-intel-tgl/unbind 2>/dev/null || true
    fi
fi
EOF
```

> **Note:** The PCI address (`0000:00:1f.3`) and driver name (`sof-audio-pci-intel-tgl`) match my laptop (Alder Lake). Replace them with the values from your own `lspci -k` output.

3. Make the script executable:

```bash
sudo chmod 755 /usr/lib/systemd/system-shutdown/fix-sof-reboot.shutdown
```

---

## Verification

Check that the script exists and has execute permissions (`x`):
```bash
ls -l /usr/lib/systemd/system-shutdown/
```

Example output (from my laptop):
```text
total 12
-rwxr-xr-x. 1 root root 332 nov  6 13:18 fix-sof-reboot.shutdown # <-- our script
-rwxr-xr-x. 1 root root 244 oct 22 19:00 fwupd.shutdown
-rwxr-xr-x. 1 root root 167 mar 16  2026 mdadm.shutdown
```

Save your work first, then reboot normally:

```bash
sudo systemctl reboot
```

> **Note:** The system should restart in complete silence, eliminating the speaker pop. In my case, it worked well.

---

## Author

Created and maintained by [@thisangelo](https://github.com/thisangelo)