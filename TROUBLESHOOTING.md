# Troubleshooting

Got an issue? Check if listed here.

## When downloading, I get `ERROR: cannot verify files.skills17.ch's certificate`

__Issue:__

You are likely behind a corporate or school proxy. On corporate or school computers,
this is often the case. If you are in a VPN, you might also be affected.

__Solution:__

We recommend to use a private computer, or turn off the VPN or Proxy.

__Hint:__

The proxy config is usually in the file `~/.docker/config`. You might have to remove the
proxy settings from there.

See [Docker documentation](https://docs.docker.com/network/proxy/) for more information.

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

The user and group of your host and Docker container probably mismatch. Therefore, the Docker container user doesn't have the permission to write to your `competitions/` directory.

__Solution:__

Get your user and group ID by running `id` in your terminal. You need to let Docker know about these IDs by adding them to the `docker-compose.yaml` file at the `competitions-manager` level. Replace `1000:1000` with your UID and GID respectively.

```yml
        ...
        volumes:
            - ./competitions:/app/data/
        user: "1000:1000"
```

## I'm getting `exec /docker-entrypoint.sh: exec format error` when running `npm start`

__Issue:__

You are likely trying to run the Docker container on an incompatible architecture.

__Solution:__

Make sure you are running the Docker container on a compatible architecture. Required images are available for both
`amd64` and `arm64` architectures, hence this is not the original of the problem. 

Try re-installing Docker and make sure you are using the correct version for your CPU architecture.

## Still having issues?

1. Check if someone else already had the same issue on the GitHub repository:
   [Issues](https://github.com/skills17/competition-manager/issues)
2. Create an issue on this GitHub repository here:
   [Create Issue](https://github.com/skills17/competition-manager/issues/new)
