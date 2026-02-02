---
title: 'Debian 13 Minimal Installation (XFCE + Bluetooth)'
description: 'This guide assumes you install Debian normally using the official installer. No fancy netinst voodoo. Just don’t click the wrong checkbox and ruin it.'
pubDate: 2026-02-02
author: 'Mage'
image: ''
tags: ['xfce', 'debian', 'linux']
---


## 1. Install Debian (The Boring Part)

Boot the Debian 13 installer and go through it as usual:

* Language, locale, keyboard — whatever
* Network — wired is easier, Wi‑Fi works too
* Disk partitioning — guided/manual, your call
* User & root password — don’t be dumb

### Important Step: Software Selection

When you reach **Software selection**:

* **Uncheck EVERYTHING**

  - [ ]  Debian desktop environment
  - [ ]  GNOME / KDE / XFCE / whatever
  - [ ]  Web server, SSH, standard system utilities

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

## 3. Install XFCE Desktop

Install XFCE **manually**, without Debian’s bloated meta-desktop nonsense:

```bash
sudo apt install xfce4 xfce4-goodies lightdm
```

Set LightDM as default display manager if asked.

Enable and start it:

```bash
sudo systemctl enable lightdm
sudo systemctl start lightdm
```

Reboot if you want to be dramatic:

```bash
sudo reboot
```

You should now boot into a clean XFCE desktop. No GNOME garbage. No KDE fireworks.

---

## 4. Install Bluetooth Support

Debian minimal = no Bluetooth, obviously.

Install required packages:

```bash
sudo apt install -y bluetooth bluez blueman
```

Enable Bluetooth service:

```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

Optional but recommended (for laptops):

```bash
sudo usermod -aG bluetooth $USER
```

Log out and back in.

---

## 5. Blueman (GUI Bluetooth Manager)

Blueman should auto-start in XFCE. If not:

* Open **Settings → Session and Startup → Application Autostart**
* Add:

  * **Name:** Blueman Applet
  * **Command:** `blueman-applet`

Now you can pair headphones, keyboards, mice like a normal human.

---

## 6. Result

You now have:

* Debian 13
* Minimal base system
* XFCE desktop (fast, boring, reliable)
* Working Bluetooth with GUI

No bloat. No mystery services. No regrets.

---

## Notes

* If you want Wi‑Fi tools later: `network-manager`
* If you want sound: `pipewire` or `pulseaudio`
* If you want fewer packages: stop installing random crap

That’s it. Keep it minimal. Keep it sane.
