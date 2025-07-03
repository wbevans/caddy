# Custom Caddy image with Cloudflare Auth and WebDav
Custom build of [Caddy](https://github.com/caddyserver/caddy) with the following plugins:
- [Cloudflare module for Caddy](https://github.com/caddy-dns/cloudflare)
- [WebDAV for Caddy](https://github.com/mholt/caddy-webdav)

## Example docker-compose.yml

```yaml
services:

  caddy:
    image: ghcr.io/wbevans/caddy:latest
    container_name: caddy
    restart: unless-stopped
    ports:
      - "443:443"
    volumes:
      - ./caddy/data:/data
      - ./caddy/config:/config
      - ./caddy/etc:/etc/caddy
      - ./caddy/webdav:/webdav

  librespeed:
    image: lscr.io/linuxserver/librespeed:latest
    container_name: librespeed
```

## Example Caddyfile

1. Create [Cloudflare API token](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)

2. Generate Caddy password hash:
```
docker run -it ghcr.io/wbevans/caddy caddy hash-password
```
3. Create ./caddy/etc/Caddyfile
```
{
    acme_dns cloudflare <cloudflare_api_key_here>
    order webdav before file_server
}

speedtest.example.com {
    reverse_proxy http://librespeed:80
}

webdav.example.com {
    root * /webdav
    basicauth {
        <userid> <password_hash>
    }
    webdav
}
```
