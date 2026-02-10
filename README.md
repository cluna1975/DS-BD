# MSSQL Server Docker Project

This project sets up a Microsoft SQL Server instance using Docker Compose.

## Important Note on Version

**Why MSSQL 2017?**
The original request was for MSSQL Server 2012. However, Microsoft SQL Server only began supporting Linux containers starting with version 2017. Version 2012 is Windows-only and cannot run in a standard Linux container (which is the default on macOS/Docker Desktop). Therefore, this project uses **MSSQL Server 2017**, as requested, which is the earliest version compatible with Linux.

## Prerequisites

- Docker
- Docker Compose

## Configuration

The database password is set in the `.env` file:
```bash
MSSQL_SA_PASSWORD=YourStrong!Passw0rd
```
*Note: Please change this password before using in production.*

## Usage

1. **Start the container:**
   ```bash
   docker-compose up -d
   ```

2. **Check status:**
   ```bash
   docker ps
   ```

3. **Connect to the database:**
   You can connect using any SQL client (like Azure Data Studio, DBeaver, or SSMS) with the following credentials:
   - **Host:** `localhost` (or `127.0.0.1`)
   - **Port:** `1433`
   - **User:** `sa`
   - **Password:** (The value from your `.env` file)

4. **Stop the container:**
   ```bash
   docker-compose down
   ```

## Volumes

Database files (`.mdf`, `.ldf`) are persisted in the local `data` directory within this project folder.
This allows you to easily access and back up your database files.

To reset the database completely, you can delete the contents of the `data` directory:
```bash
rm -rf data/*
```
