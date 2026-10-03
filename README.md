# Fieldnote Assets — sanitized Render source snapshot

This standalone source snapshot was assembled from `ZephyrianDawnstrider/asset_management_system` at source commit `2b571c6b1846c0cf3339ef043df30f31ea2bd385` on 2026-10-03. The original checkout and its history were not modified.

Only Django runtime modules, migrations, templates, static assets, the production CA bundle, dependency manifest, and build/start scripts are included. The historical SQLite database, tests, demo seeder, local data, Git metadata, secrets, logs, and development files are excluded. No database rows were copied.

## Native Render service contract

- Runtime: Python 3.12.15 (`.python-version`)
- Build: `pip install -r requirements.txt gunicorn==26.2.0 && env -u RENDER sh build.sh`
- Start: `sh start.sh`
- Health endpoint: `/health/` (application readiness endpoint; platform health-check configuration must be set separately)
- Region/plan: Singapore / Free; automatic deploys disabled

The `env -u RENDER` wrapper is required because Render sets `RENDER=true`, and the application intentionally treats that as production even during builds. Clearing it only for `build.sh` allows static collection to use disposable SQLite without contacting the configured external database. Runtime remains fail-closed and must receive an operator-provisioned PostgreSQL `DATABASE_URL`, a generated `DJANGO_SECRET_KEY`, and matching `DJANGO_ALLOWED_HOSTS` / `DJANGO_CSRF_TRUSTED_ORIGINS`. Never configure a SQLite production fallback. Build does not migrate or seed.

The application uses the included Supabase production CA bundle and strict PostgreSQL TLS verification by default. Runtime database credentials and any migrations remain under the designated database operator; they are intentionally absent from this snapshot.
