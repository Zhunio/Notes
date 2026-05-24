Self-Hosted Stack Next Steps

Tailscale

* Secure remote access to both servers
* No port forwarding needed
* Access Immich/Home Assistant privately from anywhere
* Makes VPS ↔ home server networking simple

Vaultwarden

* Central place for passwords/API keys/TOTP
* Prevents secret sprawl across notes/env files
* Easy credential sharing across services/devices

Authentik

* One login for all self-hosted apps
* Centralized MFA/security policies
* Cleaner authentication management long-term

n8n

* Connects automations across services
* Easier than maintaining scattered scripts/cron jobs
* Useful for email → automation → notifications workflows

Open WebUI + Local LLM

* Chat with your notes/configs/docs privately
* AI assistant over your infrastructure
* Useful for troubleshooting and knowledge retrieval

Infrastructure as Code

* Rebuild servers quickly if destroyed
* Keeps deployments reproducible
* Prevents “snowflake server” problems

Uptime Kuma

* Alerts when services go down
* Monitors SSL/domain expiration
* Lets you know problems before users do

Grafana + Loki

* Visibility into server/container health
* Searchable logs when debugging failures
* Helps diagnose performance/network issues quickly

ZFS / Proxmox

* Snapshots and rollback safety
* Better storage integrity/reliability
* Easier server recovery and experimentation

Restic + Backblaze B2

* Offsite encrypted backups
* Protects Immich photos + Home Assistant configs
* Recovery if home server dies/ransomware/disk failure