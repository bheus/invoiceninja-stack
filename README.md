# Invoice Ninja stack

Portainer Compose stack for Invoice Ninja on apple-pi. The app uses the official Debian image from Docker Hub. That image includes Chromium on ARM64 for SnapPDF. NGINX serves the app on host port 8012 using the checked-in configuration.

The deployment named `invoiceninja` uses four existing external volumes for the public files, app storage, MySQL, and Redis. The MySQL slow-query log still uses `/srv/invoiceninja/mysql-logs` on the host.

## Portainer migration

1. Back up Invoice Ninja with the current backup job and verify the NAS copy. Save the current stack configuration and its environment securely for rollback.
2. Configure a Git-backed Portainer stack named `invoiceninja` with `docker-compose.yml` on `main`. Enable relative-path volumes using the host path `/opt/portainer-relpath` so NGINX can read its configuration. Enable Git polling after verifying the first deployment.
3. Carry over the existing Portainer environment variables, including the application key, database and SMTP credentials, and PDF settings. Portainer supplies `stack.env` at the repository root. `stack.env.example` lists the expected core settings for reference.
4. During cutover, ensure the old stack is stopped before the Git stack starts on the same port and volumes. Preserve all four existing volumes. Keep the previous local images and the pre-cutover backup for rollback.
5. Check app health, public client portal and LAN admin access, static assets, and invoice PDF generation. If the app migrated the database, consult the pre-cutover backup before restoring the prior app version.

Dependabot opens PRs for new image tags. MySQL stays on the 8.4 series and Redis major upgrades require a separate review.
