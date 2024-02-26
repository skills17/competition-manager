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

## Still having issues?

1. Check if someone else already had the same issue on the GitHub repository:
   [Issues](https://github.com/skills17/competition-manager/issues)
2. Create an issue on this GitHub repository here:
   [Create Issue](https://github.com/skills17/competition-manager/issues/new)
