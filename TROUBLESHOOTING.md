# Troubleshooting

Use this guide to quickly diagnose common setup/runtime problems.

## Quick checks first

Run these before diving into a specific section:

```sh
docker compose ps
docker compose logs
docker compose up -d
```

## `localhost:9999` opens, but page does not load / server error

### Symptoms

- The app starts, but cannot download:
  `https://files.skills17.ch/competitions/competitions.json`
- Logs show errors like:

```txt
TypeError: fetch failed
[cause]: AggregateError
code: 'ETIMEDOUT'
```

- Or TLS/certificate errors, for example:
  `ERROR: cannot verify files.skills17.ch's certificate`

### Likely cause

The `competition-manager` container cannot reach the Internet (or specifically `files.skills17.ch`), often due to:

- school/corporate firewall restrictions
- proxy misconfiguration
- VPN interference
- DNS issues
- blocked outbound HTTPS traffic

### What to do

1. Check logs and test connectivity from inside the container:

```sh
docker compose logs competition-manager
docker compose exec competition-manager sh
curl -v https://files.skills17.ch/competitions/competitions.json
```

2. Check Docker and proxy configuration:
- `~/.docker/config.json` (see [Docker proxy docs](https://docs.docker.com/network/proxy/))
- Docker Desktop proxy settings
- `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY` (`env | grep -i proxy`)

3. Temporarily disable VPN/proxy and retry.
4. Retry from another network (for example mobile hotspot/home network).
5. If on school/corporate network, ask your admin about the specific restrictions and how to work around them.

## Container fails to start

### Symptoms

`docker compose up` exits with one or more failed services.

### What to do

1. Check service status:

```sh
docker compose ps
```

2. Check logs:

```sh
docker compose logs
```

3. Verify:
- required ports are free
- enough host and Docker memory is available
- enough host and Docker disk space is available
- network access is functional

## `EPERM` when running `node setup.js` (Windows)

### Likely cause

Missing OS permissions for local development tooling.

### What to do

Enable
[Developer Mode](https://learn.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development),
then retry `node setup.js`.

## `Permission denied` when downloading/decrypting tasks (Linux/macOS)

### Likely cause

UID/GID mismatch between host user and container user, so container cannot write to `competitions/`.

### What to do

1. Run `id` and note your UID and GID.
2. Set `user: "<uid>:<gid>"` for `competition-manager` in `docker-compose.yaml`.

Example:

```yml
services:
  competition-manager:
    volumes:
      - ./competitions:/app/data/
    user: "1000:1000"
```

## `exec /docker-entrypoint.sh: exec format error`

### Likely cause

Image/architecture mismatch (CPU architecture not matching pulled image variant).

### What to do

1. Confirm host architecture (arm64 vs amd64).
2. Re-pull images and restart:

```sh
docker compose pull
docker compose up -d
```

3. If needed, reinstall Docker Desktop with the correct architecture build.

## Still having issues?

1. Search existing issues:
   [Issues](https://github.com/skills17/competition-manager/issues?q=is%3Aissue)
2. Open a new issue:
   [Create Issue](https://github.com/skills17/competition-manager/issues/new?template=issue.yml)
