# Lead-Generation-Agent

<div align="center">

# ⚡ n8n Workflow Automation
### Self-hosted, Dockerized & Production-Ready

![n8n](https://img.shields.io/badge/n8n-powered-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-database-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)

<br/>

> 🚀 A fully self-hosted **n8n** automation server running on Docker with PostgreSQL — built for reliability, easy to deploy, and simple to maintain.

<br/>

[🚀 Quick Start](#-quick-start) • [⚙️ Configuration](#️-configuration) • [📦 Workflows](#-workflows) • [🛠️ Commands](#️-useful-commands) • [🤝 Contributing](#-contributing)

---

</div>

## 📋 Table of Contents

- [About](#-about)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Configuration](#️-configuration)
- [Project Structure](#-project-structure)
- [Workflows](#-workflows)
- [Useful Commands](#️-useful-commands)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)

---

## 🧠 About

This repo contains everything you need to spin up a **self-hosted n8n instance** in minutes. n8n is a powerful open-source workflow automation tool — think Zapier or Make, but on your own server with full control over your data.

With this setup you get:
- ✅ **Persistent storage** using PostgreSQL
- ✅ **Auto-restart** on failure or server reboot
- ✅ **Webhook support** out of the box
- ✅ **Version-controlled workflows** via exported JSON
- ✅ **Easy environment-based configuration**

---

## 🛠 Tech Stack

| Tool | Purpose |
|---|---|
| [n8n](https://n8n.io) | Workflow automation engine |
| [Docker](https://docker.com) | Containerization |
| [Docker Compose](https://docs.docker.com/compose/) | Multi-container orchestration |
| [PostgreSQL 15](https://postgresql.org) | Persistent database |

---

## ✅ Prerequisites

Make sure you have these installed:

- [Docker](https://docs.docker.com/get-docker/) `v20+`
- [Docker Compose](https://docs.docker.com/compose/install/) `v2+`

```bash
docker --version        # Docker version 20.x.x
docker compose version  # Docker Compose version v2.x.x
```

---

## 🚀 Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/your-repo.git
cd your-repo
```

### 2. Set up environment variables

```bash
cp .env.example .env
```

Open `.env` and fill in your values (see [Configuration](#️-configuration) below).

### 3. Start the services

```bash
docker compose up -d
```

### 4. Open n8n in your browser

```
http://localhost:5678
```

Create your admin account on first launch and you're good to go! 🎉

---

## ⚙️ Configuration

Copy `.env.example` to `.env` and update the values:

```env
# ── n8n ────────────────────────────────────────
N8N_HOST=localhost
N8N_PORT=5678
N8N_PROTOCOL=http
WEBHOOK_URL=http://localhost:5678/
TIMEZONE=Asia/Kolkata

# ── PostgreSQL ─────────────────────────────────
POSTGRES_USER=n8n
POSTGRES_PASSWORD=your_strong_password_here
POSTGRES_DB=n8n
```

### Environment Variables Reference

| Variable | Description | Example |
|---|---|---|
| `N8N_HOST` | Hostname where n8n runs | `localhost` or `n8n.yourdomain.com` |
| `N8N_PORT` | Port to expose n8n on | `5678` |
| `N8N_PROTOCOL` | `http` or `https` | `http` |
| `WEBHOOK_URL` | Public URL for incoming webhooks | `https://n8n.yourdomain.com/` |
| `TIMEZONE` | Your local timezone | `Asia/Kolkata` |
| `POSTGRES_USER` | PostgreSQL username | `n8n` |
| `POSTGRES_PASSWORD` | PostgreSQL password | `supersecret` |
| `POSTGRES_DB` | PostgreSQL database name | `n8n` |

> ⚠️ **Never commit your `.env` file.** It's already in `.gitignore`.

---

## 📁 Project Structure

```
├── docker-compose.yml      # Docker services definition
├── .env                    # Your local secrets (git-ignored)
├── .env.example            # Template for environment variables
├── .gitignore              # Ignores .env, data volumes, etc.
├── workflows/              # Exported n8n workflow JSON files
│   └── example-workflow.json
└── README.md
```

---

## 📦 Workflows

All workflows are exported as JSON files inside the `/workflows` folder for version control.

### Importing a workflow

1. Open n8n → click **"+"** → **"Import from file"**
2. Select any `.json` file from the `/workflows` folder

### Exporting your workflows

**From the UI:**
- Open a workflow → top-right menu → **Download**

**From the CLI:**
```bash
docker compose exec n8n n8n export:workflow --all --output=/home/node/.n8n/workflows/
```

### Using n8n's built-in Git integration *(v1.x+)*

Go to **Settings → Source Control** to connect n8n directly to this repo and push/pull workflows without manual exports.

---

## 🛠️ Useful Commands

```bash
# Start all services (detached)
docker compose up -d

# Stop all services
docker compose down

# View live logs
docker compose logs -f n8n

# Restart n8n only
docker compose restart n8n

# Pull latest n8n image
docker compose pull && docker compose up -d

# Access n8n container shell
docker compose exec n8n sh
```

---

## 🔧 Troubleshooting

**n8n not starting?**
```bash
docker compose logs n8n
```

**Can't connect to database?**
- Make sure `POSTGRES_USER`, `POSTGRES_PASSWORD`, and `POSTGRES_DB` in `.env` match what's in `docker-compose.yml`
- Wait a few seconds — PostgreSQL takes a moment to initialize on first run

**Webhooks not working?**
- Make sure `WEBHOOK_URL` is set to your **publicly accessible URL** (not `localhost`) if you're receiving webhooks from external services

**Port already in use?**
- Change `N8N_PORT` in `.env` and update the port mapping in `docker-compose.yml`

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/my-workflow`
3. Export and add your workflow JSON to `/workflows`
4. Commit: `git commit -m "Add: my awesome workflow"`
5. Push & open a Pull Request

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">

Made with ❤️ and automated with **n8n**

⭐ Star this repo if it helped you!

</div>
