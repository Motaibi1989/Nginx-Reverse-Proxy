# Nginx Reverse Proxy

A beginner-friendly and production-focused Nginx reverse proxy guide with practical Linux commands, configuration examples, testing steps, troubleshooting guidance, and a hardened custom systemd service.

## Start here

Open [`index.html`](index.html) locally or publish the repository with GitHub Pages.

## Repository structure

```text
.
├── README.md
├── index.html
├── assets/
│   ├── style.css
│   └── app.js
├── docs/
│   ├── installation.html
│   ├── linux-commands.html
│   ├── reverse-proxy.html
│   ├── systemd.html
│   └── troubleshooting.html
└── examples/
    ├── basic-reverse-proxy.conf
    └── nginx-custom.service
```

## Topics

- Nginx installation on RHEL-family and Debian-family systems
- Basic reverse proxy configuration
- Linux and Nginx administration commands
- Configuration testing and validation
- Error logs and systemd troubleshooting
- Custom `/Nginx` deployment layout
- Hardened systemd service for advanced users

## Quick test

```bash
sudo nginx -t
sudo systemctl status nginx
sudo journalctl -u nginx -n 100 --no-pager
curl -I http://localhost
```

## License

MIT
