# Laravel 11 + Octane (Swoole) on Docker – API-Only Development Environment

This repository contains a Laravel 11 API-only application running on Docker using:

- **PHP 8.4** (Alpine)
- **Laravel Octane** with **Swoole**
- **PostgreSQL** (latest)
- **Valkey** (latest) for in-memory caching
- **Docker Compose** for orchestration

---

## Quick Start

### 1. Build and start containers

```bash
./setup.sh
```
or 
```bash
./rebuild-docker.sh
```

(or manually:)

```bash
docker-compose build
docker-compose up -d
```

### 2. Install Laravel and dependencies

If not using `setup.sh`, run:

```bash
docker-compose exec app composer create-project laravel/laravel . "^11.0"
docker-compose exec app php artisan key:generate
docker-compose exec app php artisan migrate
docker-compose exec node npm install
```

### 3. Access the application

Open your browser at [http://localhost:8000/api](http://localhost:8000/api)

---

## Environment Variables

Make sure your `.env` file has the following database settings for PostgreSQL:

```env
DB_CONNECTION=pgsql
DB_HOST=db
DB_PORT=5432
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret

REDIS_CLIENT=phpredis
REDIS_HOST=valkey
REDIS_PASSWORD=null
REDIS_PORT=6379
```

---

## Running Artisan and npm commands

You can run Laravel and Node commands inside their respective containers:

```bash
docker-compose exec app php artisan migrate
docker-compose exec app php artisan db:seed
docker-compose exec app php artisan octane:start --server=swoole --host=0.0.0.0 --port=8000
```

---

## Notes

- No Nginx or Blade: This stack is designed for API-only use — minimal surface area, fast boot, clean architecture.
- No Node/Vite: Frontend asset tooling not included. Ideal for headless API services or external frontends.

---

## Troubleshooting

- If Laravel fails to connect to Postgres, confirm Postgres is running (docker-compose ps) and accepting connections.
- If Octane throws a Signals are not supported error, ensure the pcntl extension is installed and not disabled in php.ini.
- If you're running into file permission issues, try chmod -R 777 storage bootstrap/cache.

---

## License

MIT License

---

Feel free to contribute or ask for help!

## GitHub Profile

Check out more of my projects on GitHub:  
[https://github.com/mariusroyale](https://github.com/mariusroyale)