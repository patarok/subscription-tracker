# Subscription Tracker Containers

This repository currently ships a containerised development stack for the Laravel 12 codebase that lives in this repo. **We only support plain `docker compose` commands—tools such as Laravel Sail, DDEV, or other wrappers are intentionally not supported.**

## Prerequisites

- Docker Engine 24+
- Docker Compose V2 (`docker compose` CLI)
- On Linux: enable `host.docker.internal` or set `XDEBUG_CLIENT_HOST` in `.env`/compose override for Xdebug debugging

## Services

| Service | Image | Host Ports | Purpose |
|---------|-------|------------|---------|
| app | `laravel-app:latest` | _none_ | PHP-FPM container serving the Laravel application |
| queue | `laravel-app:latest` | _none_ | Queue worker process (falls back to idle mode until Laravel is installed) |
| nginx | `nginx:stable-alpine` | `8000` | HTTP entry point reverse proxying to PHP-FPM |
| db | `mysql:8.0` | _none_ (internal) | MySQL database |
| redis | `redis:7-alpine` | _none_ (internal) | Cache and queue backing store |
| mailhog | `mailhog/mailhog:v1.0.1` | `1025`, `8025` | Local SMTP capture and web UI |

## Usage

1. Build and start everything (rebuilds on changes):

   ```bash
   docker compose up --build --force-recreate
   ```

2. Verify container status:

   ```bash
   docker compose ps
   ```

   Visit http://localhost:8000 to see the Laravel welcome page. Mailhog UI is available at http://localhost:8025.

3. Stop the stack when finished:

   ```bash
   docker compose down
   ```

4. Optional: keep data by default (named volumes). To wipe MySQL data:

   ```bash
   docker compose down --volumes
   ```

## Git Hooks / Guardrails

- Enable the repository-provided hooks so commits fail if Sail artifacts are staged:

  ```bash
  git config core.hooksPath .githooks
  ```

- The `pre-commit` hook blocks commits adding `laravel/sail` to Composer files, reintroducing `vendor/laravel/sail`, or adding Sail CLI binaries. It prints guidance if triggered—fix the offending change or bypass intentionally (not recommended).

## Configuration Highlights

- Source code is mounted into the containers (`.:/var/www`) so edits on the host are reflected instantly.
- Shared Composer cache lives in the `composer-cache` volume and is scoped to `/var/www` user/group IDs controlled by `WWWUSER` / `WWWGROUP` build args.
- PHP overrides live in `docker/php/conf.d/*.ini`. Adjust `custom.ini` or `xdebug.ini` and restart containers.
- Xdebug defaults to `develop,debug` mode. Override host/port using environment variables (e.g. `XDEBUG_CLIENT_HOST=192.168.1.10`). Queue worker disables Xdebug for performance but inherits other env values.
- Queue service attempts to run `php artisan queue:work`. Until Laravel is present it falls back to an idle `tail -f /dev/null` loop so the container remains healthy.

## Helpful Commands

- Inspect Redis cache/session DBs:

  ```bash
  docker compose exec redis redis-cli -n 0 keys '*'
  docker compose exec redis redis-cli -n 1 keys '*'
  ```

- Connect to MySQL shell:

  ```bash
  docker compose exec db mysql -usubscription_user -psubscription_pass subscription_tracker
  ```

- Run ad-hoc Artisan commands (after Laravel is installed):

  ```bash
  docker compose exec app php artisan migrate
  ```

## Next Steps

- Update environment secrets (`APP_KEY`, database credentials) as required for the project.
- Add additional services (e.g., Horizon, Telescope) via Compose overrides if needed.
