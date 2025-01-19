# Docker Compose Configuration

This repository contains a Docker Compose project for setting up a multi-service development environment. Below are the details of each service included in the configuration.
MariaDB database will be stored in the "MySQL" folder. All logs (PHP-FPM and Nginx) are in the "logs" folder. **Make sure to set correct permissions to them.*

## Services

### 1. **db (MariaDB)**
- **Image**: `mariadb:latest`
- **Ports**: Exposes port `3306` for database connections.
- **Volumes**: Maps the `./mysql` directory to `/var/lib/mysql` inside the container for persistent storage.
- **Environment Variables**:
  - `MYSQL_ROOT_PASSWORD`: Root password for MariaDB.
  - `MYSQL_DATABASE`: Name of the default database.
  - `MYSQL_USER`: Database user.
  - `MYSQL_PASSWORD`: Password for the database user.

---

### 2. **web (Nginx)**
- **Build Context**: `./docker/nginx`
- **Ports**: Exposes port `80` for web server access.
- **Volumes**:
  - Maps `./src` to `/var/www/html` for web content.
  - Maps `./logs` to `/var/www/logs` for logging.
- **Links**:
  - Connects to the `php`, `db`, and `node` services.

---

### 3. **php**
- **Build Context**: `./docker/php`
- **Volumes**:
  - Maps `./src` to `/var/www/html`.
  - Maps `./logs` to `/var/www/logs`.
- **Environment Variables**:
  - `HISTFILE`: Sets the bash history file location.
  - `COMPOSER_HOME`: Defines the Composer home directory.
- **Links**:
  - Connects to the `db` service.
- **Networks**:
  - Custom aliasing via the `$ALIAS` environment variable.

---

### 4. **node**
- **Image**: `node:latest`
- **Ports**: Exposes port `5173` for development tools.
- **Volumes**:
  - Maps `./src` to `/var/www/html`.
- **Working Directory**: `/var/www/html`
- **Command**: Keeps the container running with `bash -c "tail -f > /dev/null"`.

---

### 5. **adminer**
- **Image**: `clue/adminer`
- **Environment Variables**:
  - `VIRTUAL_HOST`: Specifies the virtual host for Adminer (`dh.adminer`).
- **Ports**: Exposes port `8056` for Adminer interface.
- **Links**:
  - Connects to the `db` service as `mysql`.

---

## Optional Services (Commented Out)
### mail
- **Image**: `bytemark/smtp`
- **Restart Policy**: Always restarts the container in case of failure.

## Usage

1. Ensure you have Docker and Docker Compose installed.
2. Clone this repository.
3. Create a `.env` file in the root directory and define the following variables:
   ```env
   DB=<database_name>
   DBUSER=<database_user>
   DBPASS=<database_password>
   ALIAS=<custom_alias>
