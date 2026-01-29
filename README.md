# Rasa Studio Docker Setup

This directory contains the Docker Compose configuration to run Rasa Studio locally.

## Prerequisites

- [Docker](https://docs.docker.com/desktop/) (version 20.10.0 or higher)
- Docker Compose (version 2.0.0 or higher) (Not required if you have Docker Desktop installed)
- At least 4GB of available RAM
- The following ports must be available on your host machine:
  - 4000: Studio Backend API
  - 5005, 5006, 8000: Model Service
  - 5432: PostgreSQL Database
  - 8080: Web Client Interface
  - 8081: Keycloak Authentication
  - 9092: Kafka Broker

## Environment Variables

Before running the application, make sure to set up the following environment variables:

```bash
# Required for the Model Service
export RASA_PRO_LICENSE=your_license_key
export OPENAI_API_KEY=your_openai_key  # If using OpenAI features

# Optional - only if using custom image paths/tags
export STUDIO_IMAGE_PATH=your_custom_image_path
export STUDIO_IMAGE_TAG=your_custom_tag
```

## Running the Application

1. Navigate to this directory containing the `docker-compose.yml` file.

2. Start all services:

```bash
docker compose up
```

The startup process will show logs from all services in your terminal. The application is ready when you see the following message from the startup-helper service:

```
🚀 Rasa Studio is ready!
📱 Studio URL: http://localhost:8080
🔐 Keycloak User Management URL: http://localhost:8081/auth/
```

If you prefer to run the services in the background, you can use:

```bash
docker compose up -d
```

Then monitor the progress with:

```bash
docker compose logs -f startup-helper
```

## Accessing the Application

- Main Application: http://localhost:8080
- Keycloak Admin Console: http://localhost:8081/auth/
- Backend API: http://localhost:4000/api
- Model Service: http://localhost:8000

## Default Credentials

- Keycloak Admin:

  - Username: kcadmin
  - Password: kcadmin

- Database:
  - Username: studio
  - Password: studio

## Stopping the Application

To stop all services, press `Ctrl+C` if running in the foreground, or if running in the background:

```bash
docker compose down
```

To stop all services and remove volumes (this will delete all data):

```bash
docker compose down -v
```

## Data Persistence

The following data is persisted through Docker volumes:

- `studio-db-data`: PostgreSQL database data
- `kafka-data`: Kafka message broker data
- `rasa-pro-data`: Rasa Pro related data

## Troubleshooting

1. To view logs for a specific service:

```bash
docker compose logs -f [service-name]
```

Service names: studio-backend, studio-web-client, studio-keycloak, model-service, kafka, database

2. If you encounter memory issues, ensure Docker has enough memory allocated in your Docker Desktop settings.

## Health Checks

All services have built-in health checks. You can monitor their status using:

```bash
docker compose ps
```
