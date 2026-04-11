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

1. Copy `.env.example`:

```bash
cp .env.example .env
```

2. Generate a random secret key:

```bash
sed -i "s/SEARXNG_SECRET=.*/SEARXNG_SECRET=$(openssl rand -hex 32)/" .env
```

3. The Nginx `server_name` is composed from `SUBDOMAIN` and `DOMAIN`. By default its `searxng.local`. Change it if needed.

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
