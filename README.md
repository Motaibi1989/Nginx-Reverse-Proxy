# Nginx Reverse Proxy

A beginner-friendly and production-focused Nginx reverse proxy guide with practical Linux commands, configuration examples, testing steps, troubleshooting guidance, and a hardened custom systemd service.

## Documentation

- [Install Nginx](docs/installation.md)
- [Create a Reverse Proxy](docs/reverse-proxy.md)
- [Linux and Nginx Commands](docs/linux-commands.md)
- [Custom systemd Service](docs/systemd.md)
- [Testing and Troubleshooting](docs/troubleshooting.md)

## Examples

- [`basic-reverse-proxy.conf`](examples/basic-reverse-proxy.conf)
- [`nginx-custom.service`](examples/nginx-custom.service)

## Repository structure

```text
.
├── README.md
├── docs/
│   ├── installation.md
│   ├── reverse-proxy.md
│   ├── linux-commands.md
│   ├── systemd.md
│   └── troubleshooting.md
└── examples/
    ├── basic-reverse-proxy.conf
    └── nginx-custom.service
```

## Quick start

```bash
sudo nginx -t
sudo systemctl status nginx
sudo journalctl -u nginx -n 100 --no-pager
curl -I http://localhost
```

## Recommended workflow

1. Install Nginx.
2. Test the backend application directly.
3. Add the reverse proxy configuration.
4. Run `sudo nginx -t`.
5. Reload Nginx only after the test succeeds.
6. Check logs and connectivity when an error occurs.

## License

MIT
