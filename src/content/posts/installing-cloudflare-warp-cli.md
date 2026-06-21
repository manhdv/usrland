---
title: 'Installing Cloudflare WARP CLI on Linux'
description: 'Install and configure Cloudflare WARP CLI on Linux in just a few commands.'
pubDate: 2026-06-20
author: 'Mage'
image: ''
tags: ['cloudflare', 'warp', 'linux']
draft: false
---

# Installing Cloudflare WARP CLI on Linux

Cloudflare WARP provides a secure and privacy-focused network connection by routing your traffic through Cloudflare's global network.

This guide covers installing WARP CLI, registering your device, and connecting to the WARP network.

## Add the Cloudflare Repository

Import Cloudflare's signing key:

```bash
curl -fsSL https://pkg.cloudflareclient.com/pubkey.gpg | \
sudo gpg --yes --dearmor \
--output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg
```

Add the Cloudflare repository:

```bash
echo "deb [signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ focal main" | \
sudo tee /etc/apt/sources.list.d/cloudflare-client.list
```

## Install WARP CLI

Update package lists and install the client:

```bash
sudo apt update
sudo apt install cloudflare-warp
```

## Register the Device

Create a new WARP registration:

```bash
warp-cli registration new
```

## Connect to WARP

Connect to the Cloudflare network:

```bash
warp-cli connect
```

## Verify Connection Status

Check the current status:

```bash
warp-cli status
```

A successful connection should show that WARP is connected and active.

## Useful Commands

Connect:

```bash
warp-cli connect
```

Disconnect:

```bash
warp-cli disconnect
```

View status:

```bash
warp-cli status
```

View account information:

```bash
warp-cli account
```

Delete registration:

```bash
warp-cli registration delete
```

## Quick Start

```bash
curl -fsSL https://pkg.cloudflareclient.com/pubkey.gpg | \
sudo gpg --yes --dearmor \
--output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ focal main" | \
sudo tee /etc/apt/sources.list.d/cloudflare-client.list

sudo apt update
sudo apt install cloudflare-warp

warp-cli registration new
warp-cli connect
warp-cli status
```

Once connected, your internet traffic will be routed through Cloudflare WARP.