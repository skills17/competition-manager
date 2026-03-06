# Troubleshooting

Got an issue? Check if listed here.

## localhost:9999 opens, but the page does not load / server error shows

__Issue:__

The competition manager starts, but cannot download https://files.skills17.ch/competitions/competitions.json or other
required remote files.

Typical log output:

```
TypeError: fetch failed
[cause]: AggregateError
code: 'ETIMEDOUT'
```

Or when downloading, you get an error like this: `ERROR: cannot verify files.skills17.ch's certificate`.

Cause:

The Docker container cannot reach the Internet, or cannot reach the config host specifically. This is often caused by:

- school/corporate firewall restrictions
- proxy misconfiguration
- VPN interference
- DNS issues
- blocked outbound HTTPS traffic

__Solution:__

1. Test connectivity from inside the container:

```shell
docker compose logs competition-manager
docker exec -it <container_name> sh
curl -v https://files.skills17.ch/competitions/competitions.json
```

2. Check Docker proxy settings and environment variables:

    - `~/.docker/config.json` (See [Docker documentation](https://docs.docker.com/network/proxy/) for more information)
    - Docker Desktop proxy configuration
    - `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY` check with `env | grep -i proxy`

3. Disable VPN/proxy temporarily and retry.
4. Try another network, such as a mobile hotspot or home connection.
5. If you are on a school or corporate network, ask your administrator whether Docker containers are allowed outbound
   HTTPS access.

## Container Fails to Start

__Issue:__

One or more containers fail to start when running `docker compose up`.

__Steps:__

- Verify that necessary ports are not already in use.
- Check general network issues.
- Check you have enough memory available in Docker settings and on your host.
- Check you have enough disk space available in Docker settings and on your host.
- Check container logs with `docker compose logs [service_name]` to identify specific
  errors. Try to understand the error message and resolve the issue.

## I'm getting an `EPERM` error when running `node setup.js` and I'm on Windows

__Issue:__

You are likely running a command that requires elevated permissions.

__Solution:__

Make sure you enabled
[Developer Mode](https://learn.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development).

This is required to run Node.js and PHP commands. Otherwise, you might encounter `EPERM` errors.

## I'm getting a `Permission denied` error when downloading/decrypting the tasks, and I'm on Linux/macOS

__Issue:__

The user and group of your host and Docker container probably mismatch. Therefore, the Docker container user doesn't
have the permission to write to your `competitions/` directory.

__Solution:__

Get your user and group ID by running `id` in your terminal. You need to let Docker know about these IDs by adding them
to the `docker-compose.yaml` file at the `competitions-manager` level. Replace `1000:1000` with your UID and GID
respectively.

```yml
        ...
        volumes:
          - ./competitions:/app/data/
        user: "1000:1000"
```

## I'm getting `exec /docker-entrypoint.sh: exec format error` when running `npm start`

__Issue:__

You are likely trying to run the Docker container on an incompatible architecture for your CPU.

__Solution:__

Make sure you are running the Docker container on a compatible architecture. Required images are available for both
`amd64` and `arm64` architectures, hence this is unlikely the origin of the problem.

Try re-installing Docker and make sure you are using the correct version for your CPU architecture.

## Still having issues?

1. Check if someone else already had the same issue on the GitHub repository:
   [Issues](https://github.com/skills17/competition-manager/issues)
2. Create an issue on this GitHub repository here:
   [Create Issue](https://github.com/skills17/competition-manager/issues/new)
