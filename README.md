# SearXNG Docker Setup

A self-hosted [SearXNG](https://docs.searxng.org/) instance running behind an Nginx reverse proxy, with Valkey for caching.

## Services

| Service  | Image                    | Description            |
| -------- | ------------------------ | ---------------------- |
| `core`   | `searxng/searxng`        | SearXNG search engine  |
| `valkey` | `valkey/valkey:9-alpine` | Redis-compatible cache |
| `nginx`  | `nginx:mainline-alpine`  | Reverse proxy          |

## Prerequisites

- Docker
- Docker Compose v2

## Configuration

<details>
<summary><strong>For Linux</strong></summary>

1. Copy `.env.example`:

```bash
cp .env.example .env
```

2. Generate a random secret key:

```bash
sed -i "s/SEARXNG_SECRET=.*/SEARXNG_SECRET=$(openssl rand -hex 32)/" .env
```

</details>

<details>
<summary><strong>For Windows</strong></summary>

1. Copy `.env.example`:

```powershell
Copy-Item .env.example .env
```

2. Generate a random secret key:

```powershell
$secret = -join ((1..32) | ForEach-Object { "{0:x2}" -f (Get-Random -Max 256) })
(Get-Content .env) -replace 'SEARXNG_SECRET=.*', "SEARXNG_SECRET=$secret" | Set-Content .env
```

</details>

> The Nginx `server_name` is composed from `SUBDOMAIN` and `DOMAIN`. By default it's `searxng.local`. Change it if needed.

## Usage

**Start the container**

```bash
docker compose up -d
```

**Stop the container**

```bash
docker compose down
```

**View logs**

```bash
docker compose logs -f
```

## Accessing the Instance

Add an entry to your `/etc/hosts` (or local DNS) pointing to the host machine. (Replace `<domain-name>` with the one specified in `.env`)

```
127.0.0.1        <domain-name>
```

For example:

```
127.0.0.1        searxng.local
```

Then open `http://<domain-name>` in your browser.
