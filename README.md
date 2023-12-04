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

## Traefik example

**Don't run this example in production; it's not secure.**

The `example` directory contains an example of running the server as a
[ForwardAuth middleware in
Traefik](https://doc.traefik.io/traefik/middlewares/http/forwardauth/). It
assumes that you have Docker and Docker Compose available on your machine, and
that tailscaled is running and authenticated on the same machine (using the
tailscaled UNIX socket at the default location).

To run the example:

    cd example
    docker-compose up -d
    docker-compose logs -f

From the same machine, send an HTTP request to the proxied application using
its local IP address. You should receive a 401 error:

```
$ curl -v localhost:81/echo
*   Trying 127.0.0.1:81...
* Connected to localhost (127.0.0.1) port 81 (#0)
> GET /echo HTTP/1.1
> Host: localhost:81
> User-Agent: curl/7.81.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 401 Unauthorized
< Content-Length: 0
< Date: Mon, 04 Dec 2023 21:11:53 GMT
<
* Connection #0 to host localhost left intact

```

Now send an HTTP request using the Tailscale IP. You should now receive
a response from your echo server. Note the added `Tailscale-` fields:

```
$ curl $(tailscale ip -4):81/echo
{
  "Accept": [
    "*/*"
  ],
  "Accept-Encoding": [
    "gzip"
  ],
  "Tailscale-Login": [
    "jane-doe"
  ],
  "Tailscale-Name": [
    "Jane Doe"
  ],
  "Tailscale-Profile-Picture": [],
  "Tailscale-Tailnet": [
    "jane-doe.example"
  ],
  "Tailscale-User": [
    "jane-doe@example"
  ],
  "User-Agent": [
    "curl"
  ],
  "X-Forwarded-For": [
    "100.x.x.x"
  ],
  "X-Forwarded-Host": [
    "100.x.x.x"
  ],
  "X-Forwarded-Port": [
    "80"
  ],
  "X-Forwarded-Proto": [
    "http"
  ],
  "X-Forwarded-Server": [
    "xxxxxxxxxx"
  ],
  "X-Real-Ip": [
    "100.x.x.x"
  ]
}
```
