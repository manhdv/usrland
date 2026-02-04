---
title: 'Self-hosting Miniflux with Docker Compose + Traefik'
description: 'If you still rely on public RSS readers, you’re outsourcing something that should be boring and stable. Miniflux does one thing: read RSS efficiently, no algorithms, no noise.'
pubDate: 2026-02-04
author: 'Mage'
image: ''
tags: ['selfhost', 'miniflux']
draft: false
---

## Stack

- Docker + Docker Compose
- Traefik (reverse proxy, HTTPS, ACME)
- Miniflux
- PostgreSQL

No Kubernetes. No Helm. No unnecessary abstractions.

---

## Architecture

```

Internet
|
Traefik (80 / 443, ACME)
|
Miniflux (:8080)
|
PostgreSQL

```

---

## docker-compose.yml

Sensitive values (domains, credentials, email) are intentionally replaced.

```yaml
services:
  traefik:
    image: traefik:v3.0
    command:
      - "--api.dashboard=true"
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
      - "--certificatesresolvers.myresolver.acme.httpchallenge=true"
      - "--certificatesresolvers.myresolver.acme.httpchallenge.entrypoint=web"
      - "--certificatesresolvers.myresolver.acme.email=you@example.com"
      - "--certificatesresolvers.myresolver.acme.storage=/letsencrypt/acme.json"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./letsencrypt:/letsencrypt
    networks:
      - proxy

  miniflux:
    image: miniflux/miniflux:2.2
    depends_on:
      - miniflux-db
    environment:
      DATABASE_URL: postgres://miniflux:password@miniflux-db/miniflux?sslmode=disable
      RUN_MIGRATIONS: 1
      CREATE_ADMIN: 1
      ADMIN_USERNAME: admin
      ADMIN_PASSWORD: admin_password
    networks:
      - proxy
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.miniflux.rule=Host(`rss.example.com`)"
      - "traefik.http.routers.miniflux.entrypoints=websecure"
      - "traefik.http.routers.miniflux.tls.certresolver=myresolver"
      - "traefik.http.services.miniflux.loadbalancer.server.port=8080"

  miniflux-db:
    image: postgres:15
    environment:
      POSTGRES_USER: miniflux
      POSTGRES_PASSWORD: password
      POSTGRES_DB: miniflux
    volumes:
      - ./miniflux/db:/var/lib/postgresql/data
    networks:
      - proxy

networks:
  proxy:
    external: false
```

---

## First run (bootstrap)

```bash
docker compose up -d
```

On the first startup, Miniflux will:

* Run database migrations
* Create the admin user once

Log in and confirm everything works.

---

## Remove bootstrap credentials

After the first successful login, remove the following variables:

```yaml
CREATE_ADMIN
ADMIN_USERNAME
ADMIN_PASSWORD
```

Then restart:

```bash
docker compose up -d
```

Miniflux does **not** recreate users once the database exists.
These variables are strictly one-time bootstrap helpers.

---

## Operational notes

* Do not use `latest` tags in production
* Do not expose Miniflux ports when using Traefik
* Secure ACME storage:

  ```bash
  chmod 600 letsencrypt/acme.json
  ```
* Back up `./miniflux/db` regularly

---

## Why Miniflux

* No tracking
* No recommendation engine
* No UI clutter
* Fast and predictable

You subscribe to feeds. You read them.
That’s it.
