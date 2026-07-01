---
title: 'Build a Minimal Debian 13 XFCE Desktop'
description: 'Install a clean XFCE desktop on a minimal Debian 13 system.'
pubDate: 2026-02-02
author: 'Mage'
image: ''
tags: ['debian', 'xfce', 'linux']
---

## Overview

This guide assumes you already have a minimal Debian 13 installation without a desktop environment.

We'll install XFCE manually instead of using Debian's desktop task packages. Once the desktop is up and running, you can optionally install the XFCE extras to add more utilities and plugins.

---

## Update the System

Update your package list and upgrade existing packages:

```bash
sudo apt update
sudo apt upgrade
```

---

## Install XFCE

Install the XFCE desktop together with LightDM:

```bash
sudo apt install xfce4 lightdm
```

When prompted, select **LightDM** as the default display manager.

Enable LightDM:

```bash
sudo systemctl enable lightdm
```

Reboot the system:

```bash
sudo reboot
```

After logging in, you'll be greeted with a clean XFCE desktop.

---

## Install XFCE Goodies (Optional)

The base XFCE installation already includes everything needed for a functional desktop.

If you'd like additional applications, plugins, and utilities maintained by the XFCE project, install the **xfce4-goodies** package:

```bash
sudo apt install xfce4-goodies
```

This package includes useful components such as additional panel plugins, archive integration, sensors, media tools, and various desktop utilities.

---

## Enable Bluetooth

Install Bluetooth support:

```bash
sudo apt install bluetooth bluez blueman
```

Enable the Bluetooth service:

```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

Blueman provides a simple graphical interface for pairing and managing Bluetooth devices.

---

## Start Blueman Automatically

Blueman usually starts automatically after logging into XFCE.

If it doesn't:

**Settings → Session and Startup → Application Autostart**

Create a new entry:

- **Name:** Blueman Applet
- **Command:** `blueman-applet`

Log out and back in.

---

## Optional Packages

Depending on your workflow, you may also want to install a few additional packages.

### NetworkManager

```bash
sudo apt install network-manager network-manager-gnome
```

### Printing Support

```bash
sudo apt install system-config-printer
```

---

## Final Result

You now have:

- Debian 13
- XFCE desktop
- LightDM
- Optional XFCE Goodies
- BlueZ Bluetooth stack
- Blueman Bluetooth manager

A clean, lightweight desktop that's easy to customize and expand as needed.