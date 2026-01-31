---
title: 'Installing Canon LBP6030 UFRII LT Driver (v21.11) on Debian 13 (Trixie)'
description: 'How to install the Canon LBP6030 UFRII LT printer driver v21.11 on Debian 13 (trixie) using deprecated GTK2 packages from Debian 12 and preventing unwanted upgrades.'
pubDate: 2026-01-31
author: 'Mage'
image: ''
tags: ['printer', 'canon', 'linux']
---

> **Target system:** Debian 13 (trixie)  
> **Problem:** Canon legacy printer drivers depend on **GTK2 / libglade2**, which were removed from Debian 13.  
> **Solution:** Temporarily install required GTK2 packages from **Debian 12 (bookworm)** and lock them to prevent upgrades.

---

## Disclaimer

This guide installs **deprecated GTK2 libraries** on Debian 13 **only to support a legacy Canon printer driver**.

- Not recommended for clean or long-term systems
- Acceptable for personal or workstation use
- **Do NOT** use this approach on production servers

If your printer supports **IPP / AirPrint / Gutenprint**, use those instead.

---

## Driver Information

- Printer models: **Canon LBP6030 / LBP6030B / LBP6030w**
- Driver: **UFRII LT Printer Driver Ver. 21.11 (64-bit Linux)**
- Status: **Unmaintained / legacy**
- Dependency issue: **GTK2 removed in Debian 13**

---

## 1. Add Debian 12 (bookworm) repository (temporary)

Create a separate source list file:

```bash
sudo nano /etc/apt/sources.list.d/bookworm.list
```

Add the following line:

```
deb http://deb.debian.org/debian bookworm main
```

Update package lists:

```bash
sudo apt update
```

## 2. Install required GTK2 libraries from bookworm
Force  `apt` to install only the required packages from Debian 12:

```bash
sudo apt install -t bookworm \
  libglade2-0 \
  libgtk2.0-0 \
  libgdk-pixbuf2.0-0
```

This ensures compatible versions and proper dependency resolution.

## 3. Prevent future upgrades (IMPORTANT)
Debian 13 will later attempt to upgrade these packages, which breaks the Canon driver.
Freeze them using apt-mark hold:

```bash
sudo apt-mark hold \
  libglade2-0 \
  libgtk2.0-0 \
  libgdk-pixbuf2.0-0 \
  libgail-common \
  libgtk2.0-bin \
  libgtk2.0-common
```

Verify the hold status:

```bash
apt-mark showhold
```

## 4. Remove the bookworm repository
Once the required packages are installed and locked, remove the bookworm source:

```bash
sudo rm /etc/apt/sources.list.d/bookworm.list
sudo apt update
```

This prevents accidental cross-release upgrades.

## 5. Install CUPS and drivers:

Install `cups`:

```bash
sudo apt install cups
```

Extract the Canon driver archive and run the installer:

```bash
sudo ./install.sh
```

If the installer reports missing dependencies, resolve them and re-run the script.

## 6. Test printing
* Add the printer via CUPS

* Print a test page

* If it prints successfully, do not modify the setup further

**Notes**

* `apt-mark` hold locks the exact package versions and prevents upgrades

* This setup survives normal `apt upgrade`

* Major Debian version upgrades may require repeating this process

**Final Verdict**

This works, but it is technical debt.

GTK2 is deprecated, and Canon no longer maintains this driver.
Use this workaround only if no IPP or native alternative is available.

If it prints, stop touching it.