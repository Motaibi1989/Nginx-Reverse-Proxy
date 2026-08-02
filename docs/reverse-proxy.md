# Your First Reverse Proxy

Nginx accepts the client request and forwards it to an application server. The application can remain on a private address such as `127.0.0.1:8080`.

## Basic configuration

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Replace `example.com` with your hostname and `127.0.0.1:8080` with the backend server address.

A ready-to-use file is available at [`examples/basic-reverse-proxy.conf`](../examples/basic-reverse-proxy.conf).

## Test in the correct order

```bash
# 1. Test the backend directly
curl -I http://127.0.0.1:8080

# 2. Validate Nginx configuration
sudo nginx -t

# 3. Reload without downtime
sudo systemctl reload nginx

# 4. Test through Nginx
curl -I http://example.com
```

## Understand failures

| Test | Result | Meaning |
|---|---|---|
| Backend test fails | Connection refused or timeout | Fix the application, network, or backend port first. |
| Backend works but proxy fails | `502`, `504`, or connection error | Check `proxy_pass`, permissions, SELinux, DNS, and Nginx logs. |
| `nginx -t` fails | File and line number shown | Fix the syntax before starting or reloading Nginx. |

## Related guides

- [Installation](installation.md)
- [Linux commands](linux-commands.md)
- [Testing and troubleshooting](troubleshooting.md)
