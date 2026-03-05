# Product Service

## Overview

The Product Service is a microservice responsible for managing product catalog and product-related operations in the e-commerce platform.

## Features

- Product CRUD operations
- Product catalog management
- Integration with Kafka for event-driven architecture
- Protobuf support for efficient data serialization
- API versioning (v1)

## Technology Stack

- **Framework**: FastAPI
- **Database**: PostgreSQL
- **Message Broker**: Apache Kafka
- **ORM**: SQLModel
- **Serialization**: Protocol Buffers (Protobuf)

## Port Configuration

- **Service Port**: 8085
- **Database Port**: 5433

## API Endpoints

- **Root Path**: `/product-service`
- **Development Server**: `http://127.0.0.1:8085`
- **API Documentation**: `http://127.0.0.1:8085/docs`

## Database Configuration

- **Database Name**: product_postgres_db
- **Container Name**: ProductCont
- **User**: ziakhan
- **Host**: localhost:5433

## Running the Service

### Using Docker Compose

```bash
docker-compose up product_service
```

### Local Development

```bash
cd product
pip install -e .
uvicorn app.main:app --reload --port 8085
```

## Environment Variables

Configure the following in your environment:

- Database connection settings
- Kafka bootstrap servers
- API configurations

## Project Structure

```
product/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI application entry point
│   ├── settings.py          # Configuration settings
│   ├── stock_pb2.py         # Protobuf generated code
│   ├── stock.proto          # Protobuf definitions
│   ├── api/                 # API routes
│   ├── consumers/           # Kafka consumers
│   ├── core/                # Core utilities and database
│   ├── crud/                # CRUD operations
│   └── models/              # Database models
├── tests/                   # Unit and integration tests
├── Dockerfile.dev           # Development Docker configuration
└── pyproject.toml           # Python dependencies

```

## Dependencies

See `pyproject.toml` for full list of dependencies.

## Testing

```bash
pytest tests/
```

## Notes

- The service uses CORS middleware allowing all origins for development
- Database tables are created automatically on startup
- Integrated with Kong API Gateway
