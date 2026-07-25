> [!IMPORTANT]
> **Retired (2026-07):** replaced by the `tailscale` portal of
> [ItalyPaleAle/traefik-forward-auth](https://github.com/ItalyPaleAle/traefik-forward-auth) v4
> (see [adaricorp/adarigod#1984](https://github.com/adaricorp/adarigod/pull/1984),
> which also fixes the memory leak tracked in adaricorp/adarigod#1250).
> This repository is archived; releases remain available for installs
> that predate the swap. No further releases will be made.

# tailscale-forward-auth

A Tailscale authentication server for general use with the
[Traefik proxy ForwardAuth middleware](https://doc.traefik.io/traefik/middlewares/http/forwardauth/).

It was derived from the
[Tailscale nginx-auth command](https://github.com/tailscale/tailscale/blob/741ae9956e674177687062b5499a80db83505076/cmd/nginx-auth/README.md),
but it is decoupled from NGINX and packaged as a Docker image.

This particular project started as a fork of the
[tailscale-forward-auth example](https://github.com/kevin-hanselman/tailscale-forward-auth).

The major differences from the original project are:

- Permit tagged nodes and include a Tailscale-Acl-Tags header.

- Bring in major fixes from [nginx-auth](https://github.com/tailscale/tailscale/tree/main/cmd/nginx-auth). Thanks go to @lox for [starting this work](https://github.com/lox/tailscale-forward-auth).
