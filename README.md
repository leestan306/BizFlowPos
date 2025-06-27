# BizFlow Server Setup Guide

This guide will help you set up and run the BizFlow backend API and admin frontend using Docker Compose.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Running the Application](#running-the-application)
- [Accessing the Application](#accessing-the-application)
- [Setting a Static IP Address (Optional)](#setting-a-static-ip-address-optional)
- [BizFlow POS Client Setup](#bizflow-pos-client-setup)
- [Database Backup](#database-backup)
- [Support & LICENSE_TOKEN](#support--licensetoken)

---

## Overview

BizFlow is a business workflow management platform with a Laravel backend and an admin frontend (SPA). This setup uses Docker Compose for easy orchestration and deployment.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) (v20+ recommended)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2+ recommended)
- A Linux, macOS, or Windows machine

## Environment Setup

1. **Copy the example environment file:**
   ```sh
   cp env.example .env
   ```
2. **Edit the `.env` file** to set your desired configuration. Below are the required variables (with comments):

   ```env
   ADMIN_USER_EMAIL="mail@mail.com"      # Default admin user email
   ADMIN_USER_PASSWORD="password"         # Default admin user password
   ADMIN_USER_NAME="Test Admin"           # Default admin user name
   ADMIN_USER_CODE="Test Admin"           # Default admin user code
   ADMIN_USER_NUMERIC_SECRET_CODE="44444444" # For POS Auth
   
   LICENSE_TOKEN=                       # License token (see below to obtain)
   ```

3. **Obtain your LICENSE_TOKEN:**
   - Contact Stanley Kariuki via email at leestankariuki@gmail.com or WhatsApp at +254785017308 to request your LICENSE_TOKEN.
   - For more information or support, use the same contact details.

## Running the Application

1. **Start the services:**
   ```sh
   docker-compose up -d
   ```
2. **View logs:**
   ```sh
   docker-compose logs -f backend
   docker-compose logs -f admin
   ```
3. **Stop the services:**
   ```sh
   docker-compose down
   ```
4. **Update services:**
   ```sh
   docker-compose pull
   docker-compose up -d
   ```

## Accessing the Application

- **Backend API:** [http://localhost:8000](http://localhost:8000)
- **Admin Frontend:** [http://localhost:3003](http://localhost:3003)

> If you changed the ports in your `.env`, use those ports instead.

## Setting a Static IP Address (Optional)

To access the server from an external device (e.g., on your LAN):

1. **Find your host machine's local IP address:**
   ```sh
   ip addr show
   # or
   hostname -I
   ```
2. **Edit your `.env` file:**
   - Set `APP_URL` and `FRONTEND_API_URL` to use your host's static IP, e.g.:
     ```env
     APP_URL=http://192.168.1.100:8000
     FRONTEND_API_URL=http://192.168.1.100:8000/api
     ```
3. **(Optional) Set a static IP for Docker network:**

   - Edit `docker-compose.yml` to add a static IP under the `networks` section for the backend service:
     ```yaml
     services:
       backend:
         networks:
           bizflow_network:
             ipv4_address: 192.168.100.10
     networks:
       bizflow_network:
         driver: bridge
         ipam:
           config:
             - subnet: 192.168.100.0/24
     ```
   - Make sure the subnet does not conflict with your existing network.

4. **Restart Docker Compose:**
   ```sh
   docker-compose down
   docker-compose up -d
   ```
5. **Access from another device:**
   - Use `http://<your-static-ip>:8000` for the backend and `http://<your-static-ip>:3003` for the frontend.

## BizFlow POS Client Setup

BizFlow POS Client is a desktop application (available for Windows, Mac, and Linux) that connects to your BizFlow server.

- **Download BizFlow POS Client:** [https://bizflow.co.ke](https://bizflow.co.ke)
- **Supported platforms:** Windows, macOS, Linux

**When prompted for the server URL in the POS client:**

- Enter your backend API URL with `/api` at the end.
  - For example, if your backend is running at `http://localhost:8000`, enter:
    ```
    http://localhost:8000/api
    ```
  - If using a static IP (e.g., for LAN access), enter:
    ```
    http://192.168.1.100:8000/api
    ```
- This will allow the POS client to connect to your BizFlow server.

## Database Backup

To back up your SQLite database:

```sh
# From your host:
docker exec bizflow-backend cp /var/www/html/storage/database/database.sqlite /tmp/
docker cp bizflow-backend:/tmp/database.sqlite ./backup-$(date +%Y%m%d).sqlite
```

## Support & LICENSE_TOKEN

- **To obtain your LICENSE_TOKEN or for any support, contact:**
  - Email: leestankariuki@gmail.com
  - WhatsApp: +254785017308

---

For more information or troubleshooting, please reach out to the contact above.
