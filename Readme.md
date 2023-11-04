

# README for Docker Compose Deployment of WordPress
## Overview
This README outlines the deployment process for a WordPress site using Docker Compose, which includes services for MySQL, WordPress, Nginx, and Certbot for SSL certificate management.I also used GitHub Actions for automated deployment on commit my main branch.

## Prerequisites
- Docker and Docker Compose installed on your host machine.
- Domain name pointing to the IP address of your host machine.
- Nginx installed on your host for reverse proxy (configuration not provided here).

## Structure
- MySQL - Database service for WordPress.
- WordPress - PHP-FPM based WordPress service.
- Nginx - Internal web server to serve WordPress content.
- Certbot - SSL certificate issuance and management.

## Quick Start
1. Clone this repository to your host.
2. Navigate to the cloned directory.
3. Copy the sample .env file and edit it with your database and WordPress variables.
4. (Optional) Stage your SSL certificate using Certbot before going live.
5. Deploy your services with Docker Compose:

```
docker-compose up -d
```

6. Follow the logs to ensure everything is running smoothly:

```
docker-compose logs -f
```

7. Go to http://your-ip:81

## File Structure and Configuration
The file structure for your Dockerized WordPress setup is organized as follows:

* host-nginx-conf/ - Contains Nginx configuration files for the host machine.
* nginx-conf/ - Contains Nginx configuration files for the Dockerized Nginx service.
* .dockerignore - Lists files and directories to ignore during Docker builds.
* .gitignore - Specifies intentionally untracked files to ignore by Git (e.g., sensitive information, system-specific files).
* env.template - A template for your .env file, which should be copied and filled out with your specific environment variables.

Make sure to copy the env.template to .env and edit it with your actual values before starting your Docker Compose deployment.

Note: It's crucial to ensure that your .gitignore file is properly set up to exclude files that may contain sensitive information, such as the .env file, SSL certificates, and any other confidential data.

## Configuration
### Environment Variables
Create a .env file based on the .env.example provided in this repository with the following variables:

```
MYSQL_ROOT_PASSWORD=secret
MYSQL_USER=nicolas
MYSQL_PASSWORD=secret
```

### Persistence 

The following Docker volumes are used to ensure data persistence:

* personal_wp_dbdata - Persists MySQL database files.
* personal_wp - Persists WordPress files.
* personal_wp_certbot_etc - Persists Certbot SSL certificates and configuration.


### Networks
A Docker bridge network personal_wp_site_network is used to facilitate communication between services.

## Nginx Configuration (Internal)
The included nginx-conf directory contains the Nginx configuration for the internal Nginx service that serves WordPress through PHP-FPM.

## Setup Nginx reverse proxy on the host

Before starting your Docker containers, you must configure Nginx on your host to act as a reverse proxy. This ensures that external HTTP/HTTPS requests are correctly routed to your Dockerized Nginx service, which then serves the WordPress application.

The configuration files for the host Nginx are located within the host-nginx-conf folder. Within this directory, you will find two configuration files:

* nicolas-castellano.conf - This is the primary Nginx configuration file for when you have SSL certificates set up.
* nicolas-castellano.no-ssl.conf - Use this configuration file initially, before you have obtained SSL certificates for your domain.

To set up the reverse proxy, follow these steps:

1. Navigate to the host-nginx-conf directory.
2. If you haven't already set up SSL, start with the nicolas-castellano.no-ssl.conf configuration file. This will allow you to initially test your setup without HTTPS.
3. Once you have confirmed that your setup is working and you have obtained SSL certificates (e.g., via Certbot), switch to the nicolas-castellano.conf file for HTTPS traffic.
4. Ensure that the Nginx configuration on your host points to the internal Docker network created by your Docker Compose setup.



## Certbot
Certbot is configured to generate SSL certificates for your domain. Please ensure your domain's DNS settings are configured to point to your server's IP.


## Note for SSL Configuration
Remember to complete the SSL configuration on your host's Nginx to handle HTTPS traffic before proxying to the Docker Nginx service.

## Troubleshooting
Check container logs and ensure that all environment variables are set correctly. Ensure ports are not in use by another service on the host.

Replace [nginx-host-configuration] with your specific Nginx configuration on the host machine to reverse proxy to the Docker containers.

## Maintenance
* Regularly update your images with docker-compose pull and redeploy.
** Monitor Certbot logs for successful certificate renewals.

For any further assistance, consult the Docker Compose documentation or the respective images' documentation on Docker Hub.

Remember to back up your volumes regularly to prevent data loss.

[nginx-host-configuration] - Your Nginx configuration on the host goes here.

Please ensure that you've replaced all necessary placeholders before proceeding with the deployment.