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

1. Generate password hash:
```
docker run -it ghcr.io/wbevans/caddy caddy hash-password
```
2. Create ./caddy/etc/Caddyfile
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
        userid $2a$14$7APcTV9pn2ywUmo4uqba6uZjaspsV8R4W2SjtErI9CBob.xN9durK
    }
    webdav
}
```
