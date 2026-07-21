# Vikunja Deployment Guide

This guide explains how to deploy Vikunja in your remote development environment using Docker.

## 🚀 Quick Start

Vikunja is now successfully deployed and running! Here's what has been set up:

### Current Status
- ✅ Vikunja v0.24.6 is running on **http://localhost:3456**
- ✅ Using SQLite database for optimal performance in development
- ✅ All migrations completed successfully
- ✅ Frontend and API are both accessible

## 📁 File Structure

```
/workspace/
├── docker-compose.yml          # Main SQLite-based configuration (active)
├── docker-compose-postgres.yml # PostgreSQL configuration (backup)
├── files/                      # Vikunja file uploads and attachments
├── db/                         # SQLite database storage
└── DEPLOYMENT.md              # This guide
```

## 🔧 Configuration

### Current Configuration (SQLite)
- **URL**: http://localhost:3456
- **Database**: SQLite (stored in `./db/vikunja.db`)
- **File Storage**: `./files` directory
- **User Registration**: Enabled
- **Task Attachments**: Enabled
- **Email**: Disabled (can be configured later)

### Environment Variables
- `VIKUNJA_SERVICE_PUBLICURL`: http://localhost:3456
- `VIKUNJA_DATABASE_TYPE`: sqlite
- `VIKUNJA_DATABASE_PATH`: /db/vikunja.db
- `VIKUNJA_SERVICE_JWTSECRET`: averyLongSecretToSe33dtheDB

## 🛠️ Management Commands

### Start/Stop Services
```bash
# Start Vikunja
sudo docker compose up -d

# Stop Vikunja
sudo docker compose down

# View logs
sudo docker compose logs vikunja

# Follow logs in real-time
sudo docker compose logs -f vikunja
```

### Status Check
```bash
# Check container status
sudo docker compose ps

# Test API endpoint
curl http://localhost:3456/api/v1/info

# Test frontend
curl http://localhost:3456/
```

## 🌐 Access Vikunja

1. **Web Interface**: Open your browser and navigate to http://localhost:3456
2. **First Time Setup**: 
   - Registration is enabled - create your first user account
   - No admin user needed - the first user becomes the owner
3. **API Access**: Available at http://localhost:3456/api/v1/

## 📊 Features Available

- ✅ Task and project management
- ✅ User registration and authentication
- ✅ File attachments (up to 20MB)
- ✅ CalDAV support
- ✅ Task comments
- ✅ TOTP (Two-Factor Authentication)
- ✅ Webhooks
- ✅ API access
- ✅ Background images
- ✅ Data import (from Vikunja files, TickTick)

## 🔄 Alternative Configurations

### Switch to PostgreSQL (if needed)
```bash
# Stop current setup
sudo docker compose down

# Use PostgreSQL configuration
sudo docker compose -f docker-compose-postgres.yml up -d
```

### Backup and Restore
```bash
# Backup SQLite database
cp db/vikunja.db backup-$(date +%Y%m%d).db

# Backup files
tar -czf files-backup-$(date +%Y%m%d).tar.gz files/
```

## 🔧 Customization

### Enable Email Notifications
Add to docker-compose.yml under environment:
```yaml
VIKUNJA_MAILER_ENABLED: 1
VIKUNJA_MAILER_HOST: your-smtp-host
VIKUNJA_MAILER_PORT: 587
VIKUNJA_MAILER_USERNAME: your-email@domain.com
VIKUNJA_MAILER_PASSWORD: your-password
```

### Change Port
Modify the ports section in docker-compose.yml:
```yaml
ports:
  - "8080:3456"  # Access on port 8080 instead
```

### Disable Registration
Add to environment:
```yaml
VIKUNJA_SERVICE_ENABLEREGISTRATION: 0
```

## 🔍 Troubleshooting

### Container Issues
```bash
# Restart containers
sudo docker compose restart

# Clean restart
sudo docker compose down && sudo docker compose up -d

# Check Docker logs
sudo docker compose logs vikunja
```

### Database Issues
```bash
# Check database file
ls -la db/vikunja.db

# Reset database (⚠️ This will delete all data!)
sudo docker compose down
sudo rm -f db/vikunja.db
sudo docker compose up -d
```

### Permission Issues
```bash
# Fix file permissions
sudo chown -R 1000:1000 files/ db/
```

## 📈 Performance Tips

1. **SQLite** is perfect for single-user or small team development
2. **PostgreSQL** is recommended for production with many users
3. **File attachments** are stored locally in the `files/` directory
4. **Database backups** should be done regularly for important data

## 🔗 Useful Links

- [Vikunja Documentation](https://vikunja.io/docs/)
- [Configuration Options](https://vikunja.io/docs/configuration/)
- [API Documentation](https://try.vikunja.io/api/v1/docs)
- [Community Forum](https://community.vikunja.io/)

## 🎉 Next Steps

1. **Access the web interface** at http://localhost:3456
2. **Create your first account** (first user has admin privileges)
3. **Import existing data** if you have any (Settings → Import)
4. **Configure email notifications** if needed
5. **Set up regular backups** of your data

Your Vikunja instance is ready to use! 🚀