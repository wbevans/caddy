# Custom Caddy image with Cloudflare Auth and WebDav
Custom build of [Caddy](https://github.com/caddyserver/caddy) with the following plugins:
- [Cloudflare module for Caddy](https://github.com/caddy-dns/cloudflare)
- [WebDAV for Caddy](https://github.com/mholt/caddy-webdav)

## Pull image
```
docker pull ghcr.io/wbevans/caddy:latest
```
## Example docker-compose.yml

```yaml
services:

  caddy:
    image: ghcr.io/wbevans/caddy-oauth2:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - "443:443"
    volumes:
      - ./caddy/data:/data
      - ./caddy/config:/config
      - ./caddy/etc:/etc/caddy

  librespeed:
    image: lscr.io/linuxserver/librespeed:latest
    container_name: librespeed
```

## Example Caddyfile

### ./caddy/etc/Caddyfile

```
{
    acme_dns cloudflare <cloudflare api key>
}

host1.example.com {
	reverse_proxy http://librespeed:80
}

host2.example.com {
	reverse_proxy http://x.x.x.x:1234
}
```
