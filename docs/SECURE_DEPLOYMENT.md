# Secure Deployment Checklist

## Important
Do **not** deploy with a root password pasted into chat. Treat any credential shared in chat as compromised.

## Recommended Access Model
1. Create a non-root deploy user, e.g. `deploy`
2. Add my SSH public key to `/home/deploy/.ssh/authorized_keys`
3. Use `sudo` only where needed
4. Disable password authentication after key-based access works
5. Keep root login disabled over SSH

## Minimal Server Hardening
- `ufw allow OpenSSH`
- `ufw allow 80/tcp`
- `ufw allow 443/tcp`
- `ufw enable`
- Install `fail2ban`
- Keep packages updated
- Set automatic security upgrades if desired
- Configure backups for app + configs

## Suggested Stack
- Ubuntu/Debian VPS
- Nginx
- Node.js LTS
- PM2 or systemd service
- Let’s Encrypt

## Server Validation Checklist
- OS version known
- DNS for `dpdpact.info` points to server
- Ports 80/443 reachable
- SSH key auth works for non-root user
- Nginx installed
- Node installed
- Reverse proxy configured
- TLS certificate issued
- App directory owned by deploy user
- Secrets stored outside repo

## Deployment Flow
1. Build app locally
2. Push repo to remote or rsync files
3. Install dependencies on server
4. Configure environment variables
5. Start service
6. Put behind Nginx
7. Enable HTTPS
8. Verify headers, uptime, and logs

## If You Want Me To Deploy
Send safe access in this form:
- host/IP
- username (non-root)
- SSH key-based auth enabled
- app path
- preferred process manager (`pm2` or `systemd`)
- whether Docker is desired

## Example Setup Commands (run by you if needed)
```bash
adduser deploy
usermod -aG sudo deploy
mkdir -p /home/deploy/.ssh
chmod 700 /home/deploy/.ssh
nano /home/deploy/.ssh/authorized_keys
chmod 600 /home/deploy/.ssh/authorized_keys
chown -R deploy:deploy /home/deploy/.ssh
```

Then in `/etc/ssh/sshd_config`, eventually ensure:
```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```
Restart SSH only after confirming key login works.
