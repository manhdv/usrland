---
title: 'Installing AzerothCore with Docker'
description: 'AzerothCore provides a simple Docker setup that makes it easy to run your own Wrath of the Lich King server without manually compiling the core or configuring databases.'
pubDate: 2026-06-20
author: 'Mage'
image: ''
tags: ['docker', 'azerothcore', 'selfhosting']
draft: false
---

# Installing AzerothCore with Docker

AzerothCore provides a simple Docker setup that makes it easy to run your own Wrath of the Lich King server without manually compiling the core or configuring databases.

This guide covers:

- Installing AzerothCore
- Adding popular modules
- Configuring module settings
- Building and starting the server with Docker

## Clone AzerothCore

Create a workspace and clone the repository:

```bash
mkdir -p ~/Workspace
cd ~/Workspace

git clone --depth 1 https://github.com/azerothcore/azerothcore-wotlk.git
cd azerothcore-wotlk
```

## Install Modules

Navigate to the modules directory and clone the desired modules:

```bash
cd modules

git clone --depth 1 https://github.com/azerothcore/mod-autobalance.git
git clone --depth 1 https://github.com/ZhengPeiRu21/mod-leech.git
git clone --depth 1 https://github.com/azerothcore/mod-npc-buffer.git

cd ..
```

### Module Overview

| Module | Description |
|----------|-------------|
| mod-autobalance | Automatically scales dungeon and raid difficulty based on party size |
| mod-leech | Improves quest and experience sharing for grouped players |
| mod-npc-buffer | Adds an NPC that provides buffs and quality-of-life features |

## Build the Server

Build all Docker images:

```bash
docker compose build
```

The first build may take some time depending on your hardware and internet connection.

## Start the Server

Launch all containers in the background:

```bash
docker compose up -d
```

Check running containers:

```bash
docker ps
```

## Configure Modules

After the first build, AzerothCore generates configuration templates for installed modules.

Copy all module configuration templates:

```bash
cp env/dist/etc/modules/*.conf.dist env/dist/etc/modules/*.conf
```

Alternatively, copy them individually:

```bash
cp env/dist/etc/modules/mod-autobalance.conf.dist \
   env/dist/etc/modules/mod-autobalance.conf

cp env/dist/etc/modules/mod-leech.conf.dist \
   env/dist/etc/modules/mod-leech.conf

cp env/dist/etc/modules/mod-npc-buffer.conf.dist \
   env/dist/etc/modules/mod-npc-buffer.conf
```

Edit the configuration files as needed:

```bash
nano env/dist/etc/modules/mod-autobalance.conf
nano env/dist/etc/modules/mod-leech.conf
nano env/dist/etc/modules/mod-npc-buffer.conf
```

After making changes, rebuild and restart the server:

```bash
docker compose build
docker compose up -d
```

## Access the Worldserver Console

Attach to the worldserver container:

```bash
docker attach ac-worldserver
```

Create a GM account:

```text
account create admin password
account set gmlevel admin 3 -1
```

Detach from the console without stopping the container:

```text
CTRL+P
CTRL+Q
```

## Stop the Server

```bash
docker compose down
```

## Update AzerothCore

Pull the latest changes:

```bash
git pull
```

Update modules:

```bash
cd modules/mod-autobalance && git pull
cd ../mod-leech && git pull
cd ../mod-npc-buffer && git pull
```

Rebuild and restart:

```bash
cd ../..

docker compose build
docker compose up -d
```

## Quick Start

```bash
git clone --depth 1 https://github.com/azerothcore/azerothcore-wotlk.git

cd azerothcore-wotlk/modules

git clone --depth 1 https://github.com/azerothcore/mod-autobalance.git
git clone --depth 1 https://github.com/ZhengPeiRu21/mod-leech.git
git clone --depth 1 https://github.com/azerothcore/mod-npc-buffer.git

cd ..

docker compose build
docker compose up -d

docker attach ac-worldserver
```

At this point, AzerothCore should be running with AutoBalance, Leech, and NPC Buffer installed and ready to use.