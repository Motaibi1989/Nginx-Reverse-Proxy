# Install Nginx

Install Nginx, start the service, enable it at boot, and verify that the web server is responding.

## RHEL, Rocky Linux, or AlmaLinux

```bash
sudo dnf install nginx
sudo systemctl enable --now nginx
```

## Ubuntu or Debian

```bash
sudo apt update
sudo apt install nginx
sudo systemctl enable --now nginx
```

## Verify the installation

```bash
nginx -v
sudo nginx -t
sudo systemctl status nginx
curl -I http://localhost
```

Continue when the configuration test is successful and `curl` returns an HTTP response.

## Main locations

| Purpose | Standard path | Custom example |
|---|---|---|
| Main configuration | `/etc/nginx/nginx.conf` | `/Nginx/nginx.conf` |
| Logs | `/var/log/nginx/` | Defined in the custom configuration |
| PID | Distribution default | `/Nginx/pid/nginx.pid` |

## Next steps

- [Create your first reverse proxy](reverse-proxy.md)
- [Review Linux and Nginx commands](linux-commands.md)
- [Test and troubleshoot Nginx](troubleshooting.md)
