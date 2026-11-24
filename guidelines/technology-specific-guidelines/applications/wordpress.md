# WordPress

## Purpose

Workflow for initializing a WordPress project. WordPress is a PHP-based CMS, so "initializing" usually means standing up a local install plus a theme or plugin to develop against.

## Prerequisites

- **PHP** and **MySQL**.
- **WP-CLI** (optional but recommended).
- A local environment: **wp-env** (Docker, recommended), **Local by Flywheel**, or **XAMPP/MAMP**.

## Workflow

### 1. Start a local environment

`wp-env` is the simplest reproducible option:

```bash
npx wp-env start
```

### 2. Download and configure (WP-CLI)

```bash
wp core download
wp config create --dbname=app --dbuser=app --dbpass=secret
wp core install --url=http://localhost --title="My Site" --admin_user=admin --admin_password=admin --admin_email=you@example.com
```

### 3. Scaffold a theme or plugin

```bash
wp scaffold _s mytheme          # a starter theme
wp scaffold plugin myplugin     # a starter plugin
```

### 4. Develop

Themes live under `wp-content/themes/`, plugins under `wp-content/plugins/`.

## Notes

- WordPress is PHP — see [PHP](../languages/php.md).
- `wp-config.php` holds database credentials; keep it out of version control (use a local override).

## See Also

- [PHP](../languages/php.md)
- [PostgreSQL](../databases/postgresql.md) (or MySQL) — the database backing a WordPress site.
