---
title: 'Debian 13 Minimal Installation (Cinnamon + Bluetooth)'
description: 'This guide assumes you install Debian normally using the official installer. No fancy netinst voodoo. Just don’t click the wrong checkbox and ruin it.'
pubDate: 2026-06-30
author: 'Mage'
image: ''
tags: ['cinnamon', 'debian', 'linux']
---

## 1. Install Debian (The Boring Part)

Boot the Debian 13 installer and go through it as usual:

* Language, locale, keyboard — whatever
* Network — wired is easier, Wi-Fi works too
* Disk partitioning — guided/manual, your call
* User & root password — don’t be dumb

### Important Step: Software Selection

When you reach **Software selection**:

* **Uncheck EVERYTHING**

  - [ ] Debian desktop environment
  - [ ] GNOME / KDE / Cinnamon / XFCE / whatever
  - [ ] Web server, SSH, standard system utilities

Yes, **everything**. You want a clean, minimal system.

Finish the installation and reboot.

Congrats. You now have a black screen and a login prompt. That’s the point.

---

## 2. Login and Update System

Log in with your user (or root if you enjoy bad habits), then:

```bash
sudo apt update
sudo apt upgrade
```

---

## 3. Install Cinnamon Desktop

Install Cinnamon manually instead of Debian's desktop task packages:

```bash
sudo apt install --no-install-recommends cinnamon-core lightdm
```

If prompted, select **LightDM** as your default display manager.

Enable and start it:

```bash
sudo systemctl enable lightdm
sudo systemctl start lightdm
```

Reboot:

```bash
sudo reboot
```

You should now boot into a clean Cinnamon desktop.

---

## 4. Install Bluetooth Support

A minimal Debian installation doesn't include Bluetooth support.

Install the required packages:

```bash
sudo apt install bluetooth bluez blueman
```

Enable the Bluetooth service:

```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

(Optional) Add your user to the Bluetooth group:

```bash
sudo usermod -aG bluetooth $USER
```

Log out and log back in.

---

## 5. Blueman (GUI Bluetooth Manager)

Blueman usually starts automatically under Cinnamon.

If it doesn't:

* Open **Startup Applications**
* Add a new startup program:

  * **Name:** Blueman Applet
  * **Command:** `blueman-applet`

You can now pair Bluetooth headphones, keyboards, mice, and other devices.

---

## 6. Result

You now have:

* Debian 13
* Minimal base system
* Cinnamon desktop
* LightDM display manager
* Bluetooth support (BlueZ + Blueman)

No giant Debian desktop task packages. Just the desktop you actually wanted.

---

## Notes

* For Wi-Fi management:

```bash
sudo apt install network-manager network-manager-gnome
```

* For printing support:

```bash
sudo apt install system-config-printer
```

* Cinnamon works best with hardware acceleration, but it runs fine on most modern systems.

That's it. Keep it minimal. Keep it sane.