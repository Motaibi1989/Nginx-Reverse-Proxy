# Linux and Nginx Commands

A compact command reference for installation, service management, configuration testing, logs, networking, SSL, firewall, SELinux, and troubleshooting.

## Service management

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl status nginx
sudo systemctl enable nginx
sudo systemctl disable nginx
```

## Configuration

```bash
nginx -v
nginx -V
sudo nginx -t
sudo nginx -T
sudo nginx -t -c /Nginx/nginx.conf
sudo nginx -s reload
sudo nginx -s quit
```

`nginx -T` prints the complete active configuration, including all included files.

## Logs and processes

```bash
sudo journalctl -u nginx -f
sudo journalctl -u nginx -n 100 --no-pager
sudo journalctl -u nginx --since today
tail -f /var/log/nginx/error.log
tail -f /var/log/nginx/access.log
ps -ef | grep '[n]ginx'
pgrep -a nginx
```

## Network and HTTP tests

```bash
ss -tlnp | grep nginx
ss -ant | grep ':443'
curl -I http://localhost
curl -I https://example.com
curl -v http://127.0.0.1:8080
ip addr
dig example.com
nslookup example.com
```

## SSL tests

```bash
openssl s_client -connect example.com:443 -servername example.com

echo | openssl s_client \
  -connect example.com:443 \
  -servername example.com 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates
```

## Firewall

### firewalld

```bash
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --reload
```

### UFW

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

## SELinux

```bash
getenforce
sudo ausearch -m AVC -ts recent
```

> Do not disable SELinux as the normal fix. Review the denial and apply the correct policy or file context.

## Search and backup

```bash
sudo grep -R "proxy_pass" /etc/nginx
sudo grep -R "listen" /etc/nginx
sudo find /etc/nginx -maxdepth 3 -type f
sudo cp -a /etc/nginx /etc/nginx.backup.$(date +%F-%H%M%S)
```

## Related guides

- [Installation](installation.md)
- [Reverse proxy](reverse-proxy.md)
- [Custom systemd service](systemd.md)
- [Testing and troubleshooting](troubleshooting.md)
