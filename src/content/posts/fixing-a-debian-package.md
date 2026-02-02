---
title: 'Fixing a Debian Package'
description: 'This is a practical guide for fixing a Debian package properly — not random patching, not forking on GitHub and praying.'
pubDate: 2026-02-02
author: 'Mage'
image: ''
tags: ['package', 'debian']
draft: true
---

## 0. Assumptions

You already:

* Know basic Debian packaging
* Have `devscripts`, `build-essential`, `lintian`
* Are not afraid of `debian/`

If not, stop here. This is not a beginner doc.

---

## 1. Get the Source Package

```bash
apt source barcode
cd barcode-0.99
```

Or manually:

```bash
dget barcode_0.99-9.dsc
```

This gives you:

* Upstream source
* `debian/` directory
* A clean starting point

---

## 2. Reproduce the Bug (Mandatory)

Before touching anything:

* Build the package
* Run the code
* Confirm the bug exists

If you can’t reproduce it, you’re guessing — and guessing wastes everyone’s time.

---

## 3. Fix the Code (Not the Symptom)

Edit the **actual source**, not downstream hacks.

Example (barcode SVG output issue):

* Broken XML escaping
* Invalid SVG output

Fix it at the source level:

```c
/* example */
escape_xml(input);
```

Rules:

* No Debian-specific hacks unless unavoidable
* Minimal diff
* Obvious correctness > cleverness

---

## 4. Bump the Debian Version

Edit `debian/changelog`:

```bash
dch -i
```

Example:

```text
barcode (0.99-9.1) unstable; urgency=medium

  * Fix XML escaping for valid SVG output.

 -- Your Name <you@email>  Sun, 02 Feb 2026 12:00:00 +0000
```

Rules:

* Use `-9.1`, not `-10` (this is a Debian revision)
* One logical change per entry
* No essays

---

## 5. Build the Package

From source root:

```bash
debuild -us -uc
```

If it fails:

* Fix it
* Don’t ignore warnings
* Don’t ship broken builds

Optional but recommended:

```bash
lintian ../barcode_0.99-9.1_amd64.changes
```

Warnings are sometimes fine. Errors are not.

---

## 6. Verify the Fix

Install the built package:

```bash
sudo dpkg -i ../barcode_0.99-9.1_*.deb
```

Re-test the original bug.

If it still exists, you didn’t fix it.

---

## 7. Prepare for Review

Generate source-only changes:

```bash
debuild -S
```

You should now have:

* `.dsc`
* `.debian.tar.xz`
* `.orig.tar.*`

This is what reviewers want. Not your `.deb`.

---

## 8. Upload for Review

### Debian Salsa (Preferred)

* Fork the package repo
* Push your branch
* Open a Merge Request

### Or via Debian BTS

* Create a bug report
* Attach patch or source diff
* Reference version bump

Never upload random tarballs. Ever.

---

## 9. After Acceptance

Once merged:

* Your patch enters Debian
* Syncs to Testing
* Eventually hits Stable

Congrats. You fixed Linux instead of working around it.

---

## Final Rules (Read This Again)

* Fix upstream logic first
* Debian changes must be minimal
* Version numbers matter
* Reviewers are not your QA

If future-me is tempted to hack around a broken package locally:

Don’t.

Fix it once. Fix it properly.
