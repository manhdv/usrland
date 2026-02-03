---
title: 'Self-hosting Memos with Docker Compose + Traefik'
description: 'Memos is what a note app should be: fast, minimal, no VC brain damage. Self-host it and you actually own your notes instead of renting them.'
pubDate: 2026-02-03
author: 'Mage'
image: ''
tags: ['selfhost', 'memos']
draft: false
---

This setup puts **Memos behind Traefik**, with automatic HTTPS via ACME.  
Domain and email are **intentionally redacted** — fill in your own, don’t be lazy.

---

## Architecture (nothing fancy)

- **Traefik**: reverse proxy, SSL, routing
- **Memos**: the app
- **Docker Compose**: because clicking UIs is for pain lovers

Flow:

Internet → Traefik :443 → Memos :5230

---

## Docker Compose

```yaml
services:
  traefik:
    image: traefik:v3.0
    command:
      - "--api.dashboard=true"
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"

      # ACME / Let's Encrypt
      - "--certificatesresolvers.myresolver.acme.httpchallenge=true"
      - "--certificatesresolvers.myresolver.acme.httpchallenge.entrypoint=web"
      - "--certificatesresolvers.myresolver.acme.email=YOUR_EMAIL_HERE"
      - "--certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./letsencrypt:/letsencrypt
    networks:
      - proxy

  memos:
    image: neosmemo/memos:stable
    container_name: memos
    volumes:
      - ./memos:/var/opt/memos
    environment:
      - MEMOS_MODE=prod
      - MEMOS_ADDR=0.0.0.0
      - MEMOS_PORT=5230
      - MEMOS_DATA=/var/opt/memos
    restart: unless-stopped
    networks:
      - proxy
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.memos.rule=Host(`memos.your-domain.tld`)"
      - "traefik.http.routers.memos.entrypoints=websecure"
      - "traefik.http.routers.memos.tls.certresolver=myresolver"

networks:
  proxy:
    external: false
```

---

## Things you must understand (or you’ll suffer)

### 1. Don’t expose the Memos port

No:

```yaml
ports:
  - "5230:5230"
```

Traefik talks to it internally.
Exposing the port just invites scanners and regret.

---

### 2. Volumes = your brain

```yaml
./memos:/var/opt/memos
```

Delete the container? Fine.
Delete the volume? Congrats, you wiped your memory.
Back it up.

---

### 3. HTTP challenge means port 80 is mandatory

If HTTPS doesn’t work, it’s usually one of these:

* DNS not pointing to your server
* Port 80 blocked by firewall
* `acme.json` has wrong permissions (should be `600`)

Not Traefik’s fault. It’s yours.

---

## Run it

```bash
docker compose up -d
```

Wait ~30 seconds, then open:

```
https://memos.your-domain.tld
```

First visit → create admin → done.

---

## Common failures (and who to blame)

### No HTTPS

* DNS is wrong
* Port 80 blocked
* ACME storage permission issue

### 502 Bad Gateway

* Memos container not running
* Wrong internal port (it’s `5230`, don’t freestyle)

### Traefik doesn’t route

* `Host()` rule typo
* Containers not on the same network

---

## Why Memos is worth self-hosting

* No cloud lock-in
* Markdown, no ceremony
* Local data
* Doesn’t try to be your second brain with AI hallucinations

If all you want is **notes + thoughts**, Memos beats most bloated “productivity” toys.
