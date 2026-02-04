---
title: "Practical Markdown Guide for Astro"
description: "A hands-on guide to writing Markdown content in Astro, covering images, embeds, code blocks, and HTML."
pubDate: 2026-02-04
author: "Mage"
tags: ["markdown", "astro", "writing"]
draft: true
---

Markdown in Astro is not just Markdown.  
It’s **Markdown plus HTML**, and used correctly, it’s powerful and clean.

This page is a **practical tutorial**, not a specification.

---

## 1. Frontmatter (Required)

Every post must include frontmatter that matches your `content/config.ts` schema.

Example:

```
---
title: "My Post"
description: "Short summary"
pubDate: 2026-02-04
author: "Mage"
tags: ["astro", "markdown"]
draft: false
---
```

Notes:

* `pubDate` controls sorting
* `draft: true` hides the post
* Missing required fields means the post does not exist to Astro

---

## 2. Headings

```md
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

Guidelines:

* One `H1` per post
* Start content sections from `H2`

---

## 3. Text and Inline Formatting

```md
*italic*
**bold**
`inline code`
```

Astro renders standard Markdown without surprises.

---

## 4. Images (The Right Way in Astro)

### 4.1 Co-located images (Recommended)

Structure:

```
post-name/
├─ index.md
├─ image.jpg
```

Usage:

```md
![Alt text](./image.jpg)
```

Why this works:

* Relative paths
* No broken links when URLs change
* Images stay tied to the post

---

### 4.2 Figures with captions (HTML)

Markdown has no native caption support. Use HTML:

```html
<figure>
  <img src="./image.jpg" alt="Preview">
  <figcaption>Image caption here</figcaption>
</figure>
```

Fully supported by Astro.

---

## 5. Blockquotes

Basic quote:

```md
> This is a quote  
> **Markdown still works here**
```

With attribution:

```md
> Don't communicate by sharing memory, share memory by communicating.  
> — <cite>Rob Pike</cite>
```

---

## 6. Lists

### Ordered

```md
1. First item
2. Second item
3. Third item
```

### Unordered

```md
- Item
- Another item
```

### Nested

```md
- Fruit
  - Apple
  - Orange
```

---

## 7. Tables

Tables are supported out of the box:

```md
| Name  | Age |
|-------|-----|
| Bob   | 27  |
| Alice | 23  |
```

Inline Markdown works inside tables:

```md
| Italic | Bold | Code |
|--------|------|------|
| *i*    | **b** | `c`  |
```

---

## 8. Code Blocks

### Inline code

```md
`code`
```

### Fenced code blocks (Recommended)

```html
<!doctype html>
<html>
  <body>Test</body>
</html>
```

- Syntax highlighting is supported
- Prefer fenced blocks over raw `<pre>` tags

---

## 9. HTML Inside Markdown (The Real Power)

Astro allows raw HTML inside Markdown.

### 9.1 Video embeds (YouTube, Vimeo)

```html
<div class="video-wrapper">
  <iframe
    src="https://www.youtube-nocookie.com/embed/VIDEO_ID"
    allowfullscreen>
  </iframe>
</div>
```

---

### 9.2 Audio embeds (Spotify, SoundCloud)

```html
<iframe
  src="https://open.spotify.com/embed/track/TRACK_ID"
  width="100%"
  height="152"
  loading="lazy">
</iframe>
```

---

### 9.3 Interactive embeds (CodePen, Maps, etc.)

If it works in an `<iframe>`, it works in Astro Markdown.

---

## 10. External Scripts (GitHub Gist)

```html
<script src="https://gist.github.com/user/id.js"></script>
```

Use sparingly:

* Adds external dependencies
* Affects performance and privacy

---

## 11. Forms and UI Elements

HTML forms are allowed:

```html
<form>
  <input type="text" placeholder="Text">
  <button>Submit</button>
</form>
```

Reminder:

* Markdown pages are for content
* Avoid turning posts into web apps

---

## 12. Special Inline HTML

For things Markdown does not support:

```html
<abbr title="Graphics Interchange Format">GIF</abbr>
H<sub>2</sub>O
X<sup>2</sup>
<kbd>CTRL</kbd>+<kbd>C</kbd>
<mark>highlight</mark>
```

Use when needed, not by default.

---

## Rule of Thumb

* Markdown for content
* HTML for layout and embeds
* One post, one folder
* Images live with the post
* If Markdown falls short, use HTML without hesitation

Markdown in Astro is about clarity and durability, not decoration.

If it feels boring, you’re probably doing it right.

