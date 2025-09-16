# Teable Coolify Deployment

This repository contains the Docker Compose configuration for deploying Teable on Coolify.

## Overview

Teable is a no-code database platform that provides an Airtable-like interface with PostgreSQL backend. This deployment includes:

- **Teable Application** (ghcr.io/teableio/teable:latest)
- **PostgreSQL 15.4** for data storage
- **Redis 7.2.4** for caching

## Pre-deployment Setup

### 1. Update Environment Variables

Before deploying, **MUST** update the `.env` file:

```bash
# Update these values:
PUBLIC_ORIGIN=https://your-teable-domain.com
PUBLIC_DATABASE_PROXY=your-teable-domain.com:42345

# Generate strong passwords:
POSTGRES_PASSWORD=YourSecurePostgresPassword
REDIS_PASSWORD=YourSecureRedisPassword
SECRET_KEY=YourSecureJWTSecretKey
```

### 2. Configure Domain

In Coolify, ensure your domain is properly configured and points to your Coolify server.

## Deployment Instructions

### Option 1: Deploy via Git Repository (Recommended)

1. **Push to Git Repository**:
   ```bash
   git init
   git add .
   git commit -m "Initial Teable deployment configuration"
   git remote add origin https://github.com/yourusername/teable-coolify.git
   git push -u origin main
   ```

2. **Deploy in Coolify**:
   - Access your Coolify web interface
   - Navigate to the "Teable Self-Hosted" project
   - Click "Add New Application"
   - Select "Docker Compose" as build pack
   - Connect your Git repository
   - Configure environment variables in Coolify UI
   - Deploy

### Option 2: Direct Docker Compose (Local Development)

```bash
# Clone this repository
git clone https://github.com/yourusername/teable-coolify.git
cd teable-coolify

# Update .env file with your configurations
# Start the services
docker-compose up -d
```

## Configuration Details

### Services

- **teable**: Main application (port 3000)
- **teable-db**: PostgreSQL database
- **teable-cache**: Redis cache

### Volumes

- `teable-data`: Application data and assets
- `teable-db`: PostgreSQL data
- `teable-cache`: Redis data

### Network

All services communicate through the `teable-network` bridge network.

## Environment Variables

### Required Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `PUBLIC_ORIGIN` | Public URL for the application | `https://teable.yourdomain.com` |
| `POSTGRES_PASSWORD` | PostgreSQL password | `SecurePassword123!` |
| `REDIS_PASSWORD` | Redis password | `SecureRedisPass123!` |
| `SECRET_KEY` | JWT and session secret | `SecureJWTKey123!` |

### Optional Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `TIMEZONE` | Application timezone | `UTC` |
| `BACKEND_MAIL_HOST` | SMTP host for emails | Not set |
| `BACKEND_MAIL_PORT` | SMTP port | Not set |

## Post-Deployment

### 1. Access Teable

Once deployed, access your Teable instance at: `https://your-teable-domain.com`

### 2. Initial Setup

1. Create admin account
2. Configure workspaces
3. Set up user permissions
4. Configure email settings (optional)

### 3. Backup Considerations

Ensure regular backups of:
- PostgreSQL database (`teable-db` volume)
- Application data (`teable-data` volume)

## Troubleshooting

### Common Issues

1. **Service not starting**: Check logs in Coolify dashboard
2. **Database connection errors**: Verify PostgreSQL password and connection string
3. **Redis connection errors**: Verify Redis password and URI

### Health Checks

The deployment includes health checks for PostgreSQL and Redis. Monitor these in Coolify dashboard.

### Logs

Access logs through Coolify dashboard:
- Application logs: `teable` service
- Database logs: `teable-db` service
- Cache logs: `teable-cache` service

## Security Notes

1. **Change default passwords** in `.env` file
2. **Use strong secret keys**
3. **Configure HTTPS** through Coolify/Traefik
4. **Regular updates** of Docker images
5. **Backup sensitive data** regularly

## Support

- [Teable Documentation](https://help.teable.ai)
- [Teable GitHub](https://github.com/teableio/teable)
- [Coolify Documentation](https://coolify.io/docs)