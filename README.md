# Custom Caddy image with Cloudflare Auth and WebDav
Custom build of [Caddy](https://github.com/caddyserver/caddy) with the following plugins:
- [Cloudflare module for Caddy](https://github.com/caddy-dns/cloudflare)
- [WebDAV for Caddy](https://github.com/mholt/caddy-webdav)

## Pulling private image
### Authenticating with a personal access token (classic)

> GitHub Packages only supports authentication using a personal access token (classic).

#### Save your personal access token (classic). We recommend saving your token as an environment variable.

```
export CR_PAT=YOUR_TOKEN
```

#### Using the CLI for your container type, sign in to the Container registry service at ghcr.io.

```
$ echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin
> Login Succeeded
```

### Pull image
```
docker pull ghcr.io/wbevans/caddy:latest
```
