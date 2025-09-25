# MySQL Training Environment

A Docker-based MySQL training environment with pre-configured database schema and sample data. This setup provides a complete MySQL development environment with Adminer for database management.

## Overview

This repository contains:
- **MySQL 8.0.4** server with custom configuration optimized for training
- **Pre-loaded database schema** based on OXID eShop structure with comprehensive e-commerce tables
- **Sample data** for hands-on training exercises
- **Adminer** web interface for easy database management
- **Custom MySQL configuration** optimized for development and training

## Features

- 📚 **Complete e-commerce database schema** with tables for products, orders, users, categories, etc.
- 🔧 **Optimized MySQL configuration** for training purposes
- 🌐 **Web-based database management** via Adminer
- 🐳 **Docker containerized** for easy setup and portability
- 🔒 **Pre-configured credentials** for immediate use
- 📊 **Slow query logging** enabled for performance analysis

## Prerequisites

- Docker
- Docker Compose

## Quick Start

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ESYON-GmbH/mysql_training.git
   cd mysql_training
   ```

2. **Start the environment:**
   ```bash
   docker-compose up -d
   ```

3. **Access the services:**
   - **Adminer (Database Management)**: http://localhost:8080
   - **MySQL Server**: localhost:3306

## Database Connection Details

### Default Credentials
- **Server**: localhost (or mysql-training-mysql from within Docker)
- **Port**: 3306
- **Database**: demo
- **Username**: dev
- **Password**: dev
- **Root Password**: dev

### Adminer Login
1. Open http://localhost:8080
2. Use the following connection details:
   - **System**: MySQL
   - **Server**: mysql-training-mysql
   - **Username**: dev (or root)
   - **Password**: dev
   - **Database**: demo

## Database Schema

The training database includes a comprehensive e-commerce schema with tables for:

- **Products & Catalog**: `oxarticles`, `oxcategories`, `oxmanufacturers`, `oxvendor`
- **Users & Authentication**: `oxuser`, `oxuserbaskets`, `oxuserpayments`
- **Orders & Transactions**: `oxorder`, `oxorderarticles`, `oxpayments`
- **Content Management**: `oxcontents`, `oxlinks`, `oxnews`
- **Marketing**: `oxactions`, `oxdiscount`, `oxvouchers`
- **Configuration**: `oxconfig`, `oxshops`, `oxcountry`
- **And many more...**

## File Structure

```
mysql_training/
├── docker-compose.yaml          # Docker services configuration
├── mysql/
│   ├── Dockerfile              # MySQL container customization
│   ├── conf/
│   │   └── mysql-docker.cnf    # MySQL server configuration
│   └── dumps/
│       ├── 1_database_schema.sql  # Database structure
│       └── 2_demo_data.sql        # Sample data
├── mysql_data/                 # Persistent database storage (created automatically)
└── README.md
```

## Configuration Highlights

### MySQL Optimization
The MySQL server is configured with:
- **Character Set**: UTF-8 (utf8_general_ci)
- **InnoDB Buffer Pool**: 250MB
- **Key Buffer**: 200MB
- **Slow Query Log**: Enabled (queries > 1 second)
- **Local Infile**: Enabled for data import
- **Max Connections**: 20 (suitable for training)

### Security Note
This setup uses default credentials for training purposes. **Do not use in production environments.**

## Usage Examples

### Connect via MySQL CLI
```bash
# Using Docker
docker exec -it mysql-training-mysql mysql -u dev -pdev demo

# Or from host (if MySQL client installed)
mysql -h localhost -P 3306 -u dev -pdev demo
```

### Common Training Queries
```sql
-- Show all tables
SHOW TABLES;

-- Explore article structure
DESCRIBE oxarticles;

-- Sample data query
SELECT OXTITLE, OXPRICE FROM oxarticles LIMIT 10;

-- Check database configuration
SHOW VARIABLES LIKE 'character_set%';
```

## Maintenance Commands

### Stop Services
```bash
docker-compose down
```

### Restart Services
```bash
docker-compose restart
```

### View Logs
```bash
# All services
docker-compose logs

# MySQL only
docker-compose logs mysql-training-mysql

# Adminer only
docker-compose logs mysql-training-adminer
```

### Clean Reset
```bash
# Stop and remove containers + volumes
docker-compose down -v

# Remove persistent data
rm -rf mysql_data/

# Start fresh
docker-compose up -d
```

## Data Persistence

Database data is persisted in the `mysql_data/` directory, which is automatically created when you first start the services. This ensures your data survives container restarts.

## Troubleshooting

### Port Already in Use
If ports 3306 or 8080 are already in use, modify `docker-compose.yaml`:
```yaml
ports:
  - "3307:3306"  # Change MySQL port
  - "8081:8080"  # Change Adminer port
```

### Connection Issues
1. Ensure containers are running: `docker-compose ps`
2. Check logs: `docker-compose logs mysql-training-mysql`
3. Verify network connectivity: `docker network ls`

### Performance Issues
- Increase memory allocation in `mysql/conf/mysql-docker.cnf`
- Monitor slow queries: Check `mysql_data/slow.log`

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is for educational purposes. The database schema is based on OXID eShop structure for training purposes.