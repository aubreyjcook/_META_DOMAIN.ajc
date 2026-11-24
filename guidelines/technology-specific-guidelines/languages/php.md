# PHP

## Purpose

Workflow for initializing a PHP project in a blank repository.

## Prerequisites

- **PHP** (the interpreter).
- **Composer** (the package manager).
- A web server — Apache/Nginx, or PHP's built-in dev server.

## Workflow

### 1. Initialize the package

```bash
composer init
```

This interactively creates a `composer.json`.

### 2. Create the front controller

```plaintext
project/
├── public/
│   └── index.php     # the entry point all requests route through
├── src/
├── composer.json
└── .gitignore
```

### 3. Configure PSR-4 autoloading

In `composer.json`:

```json
{
  "autoload": {
    "psr-4": {
      "App\\": "src/"
    }
  }
}
```

Then generate the autoloader:

```bash
composer dump-autoload
```

### 4. Run the dev server

```bash
php -S localhost:8000 -t public
```

### 5. Add dependencies

```bash
composer require <package>
```

## Notes

- `vendor/` belongs in `.gitignore`; **commit** `composer.lock`.
- PSR-4 maps the `App\` namespace to the `src/` directory.

## See Also

- [WordPress](../applications/wordpress.md) — a PHP-based CMS.
