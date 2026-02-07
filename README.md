# Laravel Nixpacks Configuration

This repository contains a custom Nixpacks configuration for deploying Laravel applications with Supervisor process management.

## Compatible Platforms

This configuration works with any platform that uses Nixpacks as a build system, including:

- **Railway**
- **Coolify**
- **Dokploy**

## What This Does

This configuration sets up a production-ready Laravel deployment with:

- **Nginx** web server
- **PHP-FPM** for processing PHP requests
- **Laravel Queue Workers** (12 processes by default)
- **Supervisor** for process management and monitoring

All processes are managed by Supervisor to ensure they stay running and automatically restart if they crash.

## Files Included

- `nixpacks.toml` - Main Nixpacks configuration
- Supervisor configuration for managing multiple processes
- Nginx configuration template optimized for Laravel
- PHP-FPM pool configuration
- Laravel queue worker configuration

## Usage

### Railway
1. Add the `nixpacks.toml` file to the root of your Laravel project
2. Deploy to Railway - it will automatically detect and use this configuration
3. Set any required environment variables in Railway dashboard

### Coolify
1. Add the `nixpacks.toml` file to the root of your Laravel project
2. Create a new project in Coolify and connect your repository
3. Coolify will automatically detect and use the Nixpacks configuration
4. Configure environment variables in the Coolify project settings

### Dokploy
1. Add the `nixpacks.toml` file to the root of your Laravel project
2. Create a new application in Dokploy
3. The Nixpacks configuration will be automatically detected
4. Set environment variables in the Dokploy application settings

## Configuration Options

### Laravel Queue Workers

By default, 12 queue worker processes are configured. To reduce memory/CPU usage, modify the `numprocs` value in the `worker-laravel.conf` section:
```toml
numprocs=2
```

### PHP Upload Limits

Default limits are set in `php-fpm.conf`:
- `post_max_size = 35M`
- `upload_max_filesize = 30M`

### Environment Variables

The configuration supports these Nixpacks variables:
- `PORT` - Server port (auto-set by the platform)
- `NIXPACKS_PHP_ROOT_DIR` - Custom document root (optional)
- `NIXPACKS_PHP_FALLBACK_PATH` - Custom fallback path (optional)
- `IS_LARAVEL` - Enables Laravel-specific error handling

## How It Works

1. **Build Phase**: Sets up Supervisor configuration files
2. **Start Phase**: 
   - Processes the Nginx template with environment variables
   - Starts Supervisor in daemon mode
3. **Supervisor** manages three services:
   - Nginx (web server)
   - PHP-FPM (PHP processor)
   - Laravel queue workers (background jobs)

## Process Management

Supervisor monitors and manages all processes:
- Automatic restart on failure
- Configurable logging for each service
- Graceful shutdown handling
- Process group management for queue workers

## Logs

Logs are stored in `/var/log/`:
- `/var/log/supervisord.log` - Supervisor daemon logs
- `/var/log/worker-nginx.log` - Nginx access and error logs
- `/var/log/worker-phpfpm.log` - PHP-FPM logs
- `/var/log/worker-laravel.log` - Laravel queue worker logs

## Requirements

- Laravel application
- Deployment platform using Nixpacks (Railway, Coolify, or Dokploy)

## Credits

Nixpacks configuration for Laravel deployment with multi-process management using Supervisor.
