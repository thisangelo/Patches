# Intel SOF Reboot Audio Pop Fix (`fix_sof_reboot_pop`)

A lightweight systemd shutdown hook that eliminates electrical pop sounds (*DC Offset Pop*) on Intel laptops during system reboot.

---

## Problem

On Intel-based laptops using the open-source `sof-audio-pci-intel-tgl` audio driver, an annoying electrical click or pop sound (*DC Offset Pop*) occurs on the left speaker when the system is rebooted. 

This happens because the kernel's audio driver performs an abrupt PCI bus reset during a reboot without cleanly powering down or draining residual voltage from the audio amplifier capacitors.

> **Compatibility Note:** This is an external, hardware-dependent patch. Its necessity and effectiveness vary based on your specific laptop model and audio circuit design.

---

## Solution

A lightweight systemd shutdown script that cleanly unbinds the audio controller from the PCI bus (`0000:00:1f.3`) strictly when the system performs a reboot operation.

---

## Features

- Unbinds the `sof-audio-pci-intel-tgl` driver before system reset.
- Triggered exclusively on `reboot` events (does not interfere with standard poweroff).
- Zero dependencies, running natively via systemd shutdown hooks.

---

## Installation

1. Create the shutdown script:

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

2. Make the script executable:

```bash
sudo chmod +x /usr/lib/systemd/system-shutdown/fix-sof-reboot.shutdown
```

---

## Verification

Reboot your system normally:

```bash
sudo systemctl reboot
```

> **Note:** The system should restart in complete silence, eliminating the speaker pop.

---

## Author

Created and maintained by [@thisangelo](https://github.com/thisangelo).