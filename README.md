# PHP Nginx MySQL/MariaDB Docker Stack

A complete LEMP (Linux, Nginx, MySQL/MariaDB, PHP) stack using Docker Compose with support for both MySQL and MariaDB databases.

## Prerequisites

- Docker
- Docker Compose

## Project Structure

```
.
├── docker-compose.yml
├── .env
├── mysql/
│   └── my.cnf
├── nginx/
│   └── default.conf
├── php/
│   └── Dockerfile
└── src/
    └── index.php
```

## Quick Start

### 1. Create Environment File

Create a `.env` file in the root directory:

```env
DB_ROOT_PASS=your_secure_root_password
DB_USER=appuser
DB_PASS=your_secure_password
DB_NAME=application_db
```

### 2. Build and Start Services

```
# Build and start all containers
docker compose up -d --build

# View running containers
docker compose ps

# View logs
docker compose logs -f
```

## Access Points

- **Web Application**: <http://localhost:8080>
- **phpMyAdmin**: <http://localhost:8081>
- **MySQL**: localhost:33060
- **MariaDB**: localhost:33061

## phpMyAdmin Usage

1. Open <http://localhost:8081>
2. Select server: `mysql` or `mariadb`
3. Login with credentials from `.env` file

## Database Connection

### From PHP Application

**MySQL:**

```php
$host = 'mysql';
$port = 3306;
$dbname = 'application_db';
$user = 'appuser';
$password = 'your_secure_password';
```

**MariaDB:**

```php
$host = 'mariadb';
$port = 3306;
$dbname = 'application_db';
$user = 'appuser';
$password = 'your_secure_password';
```

### From External Tools

**MySQL:**

```
Host: localhost
Port: 33060
User: appuser or root
Password: (from .env)
```

**MariaDB:**

```
Host: localhost
Port: 33061
User: appuser or root
Password: (from .env)
```

## Common Commands

### Start Services

```bash
docker-compose up -d
```

### Stop Services

```bash
docker-compose down
```

### View Logs

```bash
docker-compose logs -f
docker-compose logs -f nginx
docker-compose logs -f php
docker-compose logs -f mysql
```

### Restart Services

```bash
docker-compose restart
```

### Rebuild Services

```bash
docker-compose up -d --build
```

### Access Container Shell

```bash
docker-compose exec php bash
docker-compose exec mysql bash
docker-compose exec nginx sh
```

## Data Persistence

Data is stored in Docker volumes:

- `mysql-data`: MySQL database files
- `mariadb-data`: MariaDB database files
- `session-data`: phpMyAdmin sessions

To remove volumes (⚠️ this will delete all data):

```bash
docker-compose down -v
```

## Backup Database

### MySQL

```bash
docker-compose exec mysql mysqldump -u root -p application_db > backup.sql
```

### MariaDB

```bash
docker-compose exec mariadb mysqldump -u root -p application_db > backup.sql
```

## Restore Database

### MySQL

```bash
docker-compose exec -T mysql mysql -u root -p application_db < backup.sql
```

### MariaDB

```bash
docker-compose exec -T mariadb mysql -u root -p application_db < backup.sql
```

## Security Notes

⚠️ **This setup is for development only!**

For production:

- Use strong passwords
- Don't expose database ports externally
- Use Docker secrets instead of environment variables
- Enable SSL/TLS
- Restrict phpMyAdmin access
- Keep images updated
