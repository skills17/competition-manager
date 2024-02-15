# Troubleshooting

Got an issue? Check if listed here.

## When downloading, I get `ERROR: cannot verify files.skills17.ch's certificate`

You are likely behind a corporate proxy that intercepts SSL traffic. On corporate computers, this is often the case.
If you are in a VPN, you might also be affected.

We recommend to use a private computer, or turn off the VPN or Proxy.

Hint, the proxy config is usually in the file `~/.docker/config`.
See [Docker documentation](https://docs.docker.com/network/proxy/) for more information.
