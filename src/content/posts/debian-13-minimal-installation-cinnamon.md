---
title: 'Build a Minimal Debian 13 Cinnamon Desktop'
description: 'Skip Debian task packages and build a clean Cinnamon desktop from a minimal installation.'
pubDate: 2026-06-30
author: 'Mage'
image: ''
tags: ['debian', 'cinnamon', 'linux']
---

## Overview

This guide starts from a **minimal Debian 13 installation**—no desktop environment, no extra utilities, just a shell.

Instead of installing Debian's Cinnamon task package, we'll install only the desktop itself and add the pieces we actually need.

The result is a clean, lightweight Cinnamon system that's easy to understand and maintain.

---

## Update the System

Before installing anything:

```bash
sudo apt update
sudo apt upgrade
```

---

## Install Cinnamon Core

Install the core desktop without Debian's recommended packages:

```bash
sudo apt install --no-install-recommends cinnamon-core lightdm
```

When prompted, choose **LightDM** as the default display manager.

Enable it:

```bash
sudo systemctl enable lightdm
```

Reboot:

```bash
sudo reboot
```

After logging in, you'll have a functional Cinnamon desktop without all the extras bundled by the task package.

---

## Enable Bluetooth

Install Bluetooth support:

```bash
sudo apt install bluetooth bluez blueman
```

Enable the service:

```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

Blueman provides a simple graphical interface for pairing devices.

---

## Starting Blueman Automatically

Normally Cinnamon starts Blueman by itself.

If not:

**Menu → Startup Applications**

Add a startup entry:

- **Name:** Blueman Applet
- **Command:** `blueman-applet`

Log out and back in.

---

## Optional Packages

A minimal installation intentionally leaves out many everyday tools.

Install only what you need.

### NetworkManager

```bash
sudo apt install network-manager network-manager-gnome
```

### Printing

```bash
sudo apt install system-config-printer
```

### Archive Support

```bash
sudo apt install file-roller
```

### Extra Terminal

```bash
sudo apt install gnome-terminal
```

(Cinnamon already ships with its own terminal, so this is purely optional.)

---

## Why `cinnamon-core`?

Debian provides several ways to install Cinnamon.

For a minimal setup, `cinnamon-core` is usually the better choice because it installs only the desktop environment instead of an entire software collection.

Adding `--no-install-recommends` keeps the installation even smaller by avoiding optional packages that you may never use.

You stay in control of what gets installed.

---

## Final Result

You now have:

- Debian 13
- Cinnamon desktop
- LightDM
- BlueZ Bluetooth stack
- Blueman Bluetooth manager

No task packages.

No unnecessary applications.

Just a clean Cinnamon desktop ready for whatever comes next.