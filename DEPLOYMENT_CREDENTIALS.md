# WashPark Booking System - Deployment Credentials

## SSH Configuration

These are the deployment credentials for the WashPark Booking System.

### Required Environment Variables

```bash
# SSH Connection Details
SSH_HOST=your-vps-ip-or-hostname
SSH_USER=ubuntu
SSH_PORT=22

# Deployment Configuration
DEPLOY_PATH=/var/www/washpark
PM2_APP=washpark-booking
```

## Setup Instructions

### 1. SSH_HOST
- **Description**: Your VPS IP address or hostname
- **Example**: `203.0.113.10` or `washpark.example.com`
- **How to get**: Contact your VPS provider or check your server dashboard

### 2. SSH_USER
- **Description**: SSH username for server access
- **Default**: `ubuntu` (for Ubuntu servers)
- **Alternative**: `root`, `admin`, or custom username
- **How to get**: Check with your VPS provider

### 3. SSH_PORT
- **Description**: SSH connection port
- **Default**: `22`
- **Note**: May be different if you've configured custom SSH port for security

### 4. DEPLOY_PATH
- **Description**: Directory path where application will be deployed
- **Default**: `/var/www/washpark`
- **Note**: Ensure this directory exists and has proper permissions

### 5. PM2_APP
- **Description**: PM2 process name for the application
- **Default**: `washpark-booking`
- **Note**: This name will be used to manage the application with PM2

## GitHub Actions Secrets Setup

To use these credentials in GitHub Actions, add them as repository secrets:

1. Go to repository **Settings** > **Secrets and variables** > **Actions**
2. Click **New repository secret**
3. Add each of the following secrets:
   - `SSH_HOST`
   - `SSH_USER`
   - `SSH_PORT`
   - `DEPLOY_PATH`
   - `PM2_APP`
   - `SSH_PRIVATE_KEY` (your private SSH key for authentication)

## Example GitHub Actions Workflow

```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to Server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ${{ secrets.SSH_USER }}
          port: ${{ secrets.SSH_PORT }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd ${{ secrets.DEPLOY_PATH }}
            git pull origin main
            npm install
            pm2 restart ${{ secrets.PM2_APP }}
```

## Security Notes

⚠️ **IMPORTANT**: Never commit actual credentials to the repository!

- Store sensitive information in GitHub Secrets
- Use SSH keys for authentication instead of passwords
- Keep private keys secure and never share them
- Rotate credentials regularly
- Use environment-specific configurations

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [PM2 Documentation](https://pm2.keymetrics.io/)
- [SSH Key Generation Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

---

**Last Updated**: November 6, 2025
**Maintainer**: sachindpdev
