# n8n-automation-beginner

This repo is a practical guide to learning n8n. You'll learn how to deploy on a VPS (e.g., DigitalOcean) using Docker, set up a free domain, and explore sample workflows. Included are two examples: one for automating posts to X (Twitter) and another for building a simple Telegram bot.

## Example Workflow

Here's an example of a Telegram bot workflow integrated with AI capabilities:

![n8n Telegram Bot Workflow](https://raw.githubusercontent.com/NormanSamsudin/n8n-automation-beginner/refs/heads/main/teegram_workflow.png)

*This workflow demonstrates a Telegram bot that uses AI Agent with Google Gemini Chat Model, Simple Memory, and stock data integration.*

## Server Setup Instructions

Follow these steps to set up n8n on your server:

### 1. Connect to Server

Connect to your VPS using SSH:

```bash
ssh root@your-server-ip
```

### 2. Update Server

Update your server packages:

```bash
sudo apt update && sudo apt upgrade -y
```

### 3. Install Docker

Install Docker on your server:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh
```

### 4. Create and Change Permission

Create the n8n directory and set proper permissions:

```bash
mkdir -p ~/.n8n
sudo chown -R 1000:1000 ~/.n8n
```

### 5. Install Caddy

Install Caddy web server for reverse proxy:

```bash
apt install -y debian-keyring debian-archive-keyring apt-transport-https curl && \
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg && \
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list && \
apt update && \
apt install caddy
```

### 6. Update Caddy Configuration

Configure Caddy for your domain:

```bash
nano /etc/caddy/Caddyfile
```

Enable and restart Caddy:

```bash
systemctl enable caddy
systemctl restart caddy
```

### 7. Get Docker Image and Run Docker

Run n8n with Docker:

```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  -e N8N_SECURE_COOKIE=false \
  -e WEBHOOK_URL=https://yourdomainname.duckdns.org/ \
  --dns 8.8.8.8 \
  --dns 1.1.1.1 \
  n8nio/n8n
```

### 8. Basic Docker Commands

Useful Docker commands for managing your n8n instance:

**Restart Docker service (sometimes needed):**

```bash
sudo systemctl restart docker
```

**Check Docker containers:**

```bash
docker ps
```

**Stop n8n container:**

```bash
docker stop n8n
```

**Remove n8n container:**

```bash
docker rm n8n
```