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
