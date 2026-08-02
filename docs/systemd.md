# Custom Nginx systemd Service

Use this service when the main Nginx configuration and PID file are stored under `/Nginx` instead of the distribution defaults.

> **Important:** The `pid` directive in `/Nginx/nginx.conf` must be `pid /Nginx/pid/nginx.pid;`, and the directory must exist before startup.

## Service file

```ini
[Unit]
Description=Custom Nginx Web Server
Documentation=https://nginx.org/en/docs/
After=network-online.target local-fs.target
Wants=network-online.target

# Prevent infinite restart loops
StartLimitIntervalSec=300
StartLimitBurst=5

[Service]
Type=forking

# Custom PID file
PIDFile=/Nginx/pid/nginx.pid

# Validate configuration before startup
ExecStartPre=/usr/sbin/nginx -t -c /Nginx/nginx.conf

# Start Nginx
ExecStart=/usr/sbin/nginx -c /Nginx/nginx.conf

# Reload configuration without downtime
ExecReload=/usr/sbin/nginx -s reload -c /Nginx/nginx.conf

# Graceful shutdown
ExecStop=/usr/sbin/nginx -s quit -c /Nginx/nginx.conf

# Restart on crash
Restart=on-failure
RestartSec=5

# Allow workers to stop correctly
KillMode=mixed

# Wait before forcing stop
TimeoutStopSec=15

# High connection limits
LimitNOFILE=65535

# Security hardening
PrivateTmp=true
ProtectSystem=full
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

The repository copy is available at [`examples/nginx-custom.service`](../examples/nginx-custom.service).

## Install the service

```bash
sudo install -d -m 0755 /Nginx/pid
sudo cp examples/nginx-custom.service /etc/systemd/system/nginx-custom.service
sudo systemctl daemon-reload
sudo systemctl enable nginx-custom.service
sudo systemctl start nginx-custom.service
```

## Validate and operate

```bash
sudo /usr/sbin/nginx -t -c /Nginx/nginx.conf
sudo systemctl status nginx-custom.service --no-pager
sudo journalctl -u nginx-custom.service -n 100 --no-pager
sudo systemctl reload nginx-custom.service
sudo systemctl stop nginx-custom.service
```

## Security and compatibility notes

- `ProtectSystem=full` makes system directories read-only. Confirm that all custom log, cache, temporary, and PID paths remain writable.
- `PrivateTmp=true` gives the service private temporary directories.
- `NoNewPrivileges=true` prevents processes from gaining new privileges.
- The Nginx binary path may differ for manually compiled installations. Confirm it with `command -v nginx`.
- A package-managed Nginx service may already exist. Do not run both services on the same ports.

## Related guides

- [Installation](installation.md)
- [Linux commands](linux-commands.md)
- [Testing and troubleshooting](troubleshooting.md)
