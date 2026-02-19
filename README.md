# Semaphore UI - Ansible Web Interface

This directory contains Docker Compose configurations for running Semaphore UI, a modern web interface for Ansible playbooks.

## Quick Start

### Option 1: Simple Setup (SQLite - Testing)

```bash
# Start with SQLite database (good for testing)
docker compose -f docker-compose.simple.yml up -d

# Access at http://localhost:3000
# Default login: admin / changeme123
```

### Option 2: Production Setup (MySQL)

```bash
# Start with MySQL database (recommended for production)
docker compose up -d

# Access at http://localhost:3000
# Default login: admin / SemaphoreAdmin123!
```

## Files Overview

- `docker-compose.yml` - Production setup with MySQL database
- `docker-compose.simple.yml` - Simple setup with SQLite (for testing)
- `.env` - Environment variables (customize before production use)
- `.env.example` - Template for environment variables
- `requirements.txt` - Python packages for Ansible modules

## Configuration

1. **Copy and customize environment file** (optional):

   ```bash
   cp .env.example .env
   # Edit .env with your preferred settings
   ```

2. **Update default credentials** in `.env`:

   ```bash
   SEMAPHORE_ADMIN_PASSWORD=your_secure_password
   MYSQL_PASSWORD=your_mysql_password
   ```

3. **Generate new encryption key** (recommended for production):

   ```bash
   head -c32 /dev/urandom | base64
   # Update SEMAPHORE_ACCESS_KEY_ENCRYPTION in .env
   ```

## Usage Commands

### Start Services

```bash
# Production (MySQL)
docker compose up -d

# Simple (SQLite)
docker compose -f docker-compose.simple.yml up -d
```

### View Logs

```bash
docker compose logs -f semaphore
docker compose logs -f mysql
```

### Stop Services

```bash
docker compose stop
```

### Remove Services and volumes

```bash
docker compose down -v
```

### Update to Latest Version

```bash
docker compose pull
docker compose up -d
```

### Backup Data

```bash
# Backup MySQL data
docker compose exec mysql mysqldump -u semaphore -p semaphore > backup.sql

# Backup configuration
docker cp semaphore-ui:/etc/semaphore ./semaphore-config-backup
```

## First Time Setup

1. Access [http://localhost:3000](http://localhost:3000)
2. Login with admin credentials from `.env` file
3. **Change default passwords immediately**
4. Configure your setup:
   - **Key Store**: Add SSH keys for target servers
   - **Inventory**: Add your server inventory files
   - **Repositories**: Connect your Ansible playbook repositories
   - **Task Templates**: Create templates for your playbooks

## Network Access

- **Web Interface**: [http://localhost:3000](http://localhost:3000)
- **Change port**: Edit `SEMAPHORE_PORT` in `.env` file
- **External access**: Update `SEMAPHORE_WEB_HOST` in `.env`

## Troubleshooting

### Common Issues

1. **Port 3000 already in use**:

   ```bash
   # Change port in .env file
   SEMAPHORE_PORT=3001
   ```

2. **Database connection issues**:

   ```bash
   # Check MySQL logs
   docker compose logs mysql

   # Restart services
   docker compose restart
   ```

3. **Permission issues**:

   ```bash
   # Check container logs
   docker compose logs semaphore
   ```

### Reset Everything

```bash
# Stop and remove everything (CAUTION: This deletes all data)
docker compose down -v
docker compose up -d
```

## Security Notes

- Change all default passwords before production use
- Consider using Docker secrets for sensitive data
- Configure firewall rules to restrict access
- Enable HTTPS in production environments
- Keep containers updated regularly

## Support

- [Official Documentation](https://semaphoreui.com/docs/)
- [GitHub Repository](https://github.com/semaphoreui/semaphore)
- [Discord Community](https://discord.gg/5UXJYb2)

## Version Information

- Semaphore UI: Latest (production) / v2.16.51 (simple)
- MySQL: 8.0
- Supported tools: Ansible, Terraform, OpenTofu, PowerShell
