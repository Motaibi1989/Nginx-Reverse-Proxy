# Testing and Troubleshooting

Use this sequence to separate configuration, service, network, backend, reverse-proxy, and SSL problems.

## 1. Validate the configuration

```bash
sudo nginx -t
sudo nginx -t -c /Nginx/nginx.conf
sudo nginx -T
```

Expected result:

```text
syntax is ok
configuration test is successful
```

## 2. Check the service

```bash
sudo systemctl status nginx --no-pager
sudo systemctl is-active nginx
sudo systemctl is-enabled nginx
pgrep -a nginx
ss -tlnp | grep nginx
```

## 3. Read the errors

```bash
sudo journalctl -u nginx -n 100 --no-pager
sudo journalctl -u nginx -f
sudo journalctl -u nginx --since today
tail -n 100 /var/log/nginx/error.log
tail -f /var/log/nginx/error.log
```

For a custom deployment, check the `error_log` path configured in `/Nginx/nginx.conf`.

## 4. Test the backend directly

```bash
curl -v http://127.0.0.1:8080
curl -I http://127.0.0.1:8080
nc -vz 127.0.0.1 8080
```

If this test fails, fix the application, backend address, port, firewall, or routing before changing Nginx.

## 5. Test through Nginx

```bash
curl -v http://localhost
curl -I http://example.com
curl -vk https://example.com
openssl s_client -connect example.com:443 -servername example.com
```

## 6. Reload safely

```bash
sudo nginx -t && sudo systemctl reload nginx
```

## Common errors

| Error | Likely cause | What to check |
|---|---|---|
| `502 Bad Gateway` | Backend unavailable or incorrect `proxy_pass` | Test the backend directly and inspect the error log. |
| `503 Service Unavailable` | Upstream unavailable or application maintenance | Check upstream processes and health. |
| `504 Gateway Timeout` | Backend slow or unreachable | Check response time, routing, and proxy timeouts. |
| Connection refused | No service listening on the target port | Use `ss -tlnp` and start the backend. |
| Host not found in upstream | DNS or hostname error | Use `dig`, `getent hosts`, and review `proxy_pass`. |
| Permission denied | Filesystem permissions or SELinux denial | Check ownership, modes, `ausearch`, and contexts. |
| Address already in use | Another process owns port 80 or 443 | Run `ss -tlnp \| grep -E ':80\|:443'`. |
| SSL certificate error | Wrong certificate, key, chain, hostname, or expiry | Use `openssl s_client` and inspect certificate dates. |

## Recommended diagnostic bundle

```bash
sudo nginx -t
sudo systemctl status nginx --no-pager
sudo journalctl -u nginx -n 100 --no-pager
ss -tlnp | grep nginx
curl -v http://127.0.0.1:8080
curl -v http://localhost
```

## Related guides

- [Installation](installation.md)
- [Reverse proxy](reverse-proxy.md)
- [Linux commands](linux-commands.md)
- [Custom systemd service](systemd.md)
