# Inventory Service

## Overview

The Inventory Service manages stock levels and inventory operations for the e-commerce platform. It listens to product and order events to automatically update stock quantities.

## Features

- Real-time inventory tracking
- Automatic stock updates based on orders
- Stock addition via product events
- Integration with Order and Product services
- Event-driven architecture with Kafka consumers

## Technology Stack

- **Framework**: FastAPI
- **Database**: PostgreSQL
- **Message Broker**: Apache Kafka
- **ORM**: SQLModel
- **Serialization**: Protocol Buffers (Protobuf)
- **Type Checking**: MyPy

## Port Configuration

- **Service Port**: 8086
- **Database Port**: 5434

## API Endpoints

- **Root Path**: `/inventory-service`
- **Development Server**: `http://127.0.0.1:8086`
- **API Documentation**: `http://127.0.0.1:8086/docs`

## Database Configuration

- **Database Name**: inventory_postgres_db
- **Container Name**: InventoryCont
- **User**: ziakhan
- **Host**: localhost:5434

## Kafka Topics

The service consumes from the following topics:

- **add_stock**: For adding new inventory items
- **update_stock**: For updating stock quantities based on orders

## Running the Service

### Using Docker Compose

```bash
docker-compose up inventory_service
```

### Local Development

```bash
cd inventory
pip install -e .
uvicorn app.main:app --reload --port 8086
```

## Project Structure

```
inventory/
├── app/
│   ├── __init__.py
│   ├── main.py                  # FastAPI application entry point
│   ├── settings.py              # Configuration settings
│   ├── stock_pb2.py             # Protobuf generated code
│   ├── stock.proto              # Protobuf definitions
│   ├── api/
│   │   ├── deps.py              # API dependencies
│   │   └── routes/              # API route handlers
│   ├── consumer/
│   │   ├── order_consummer.py   # Order event consumer
│   │   └── product_consumer.py  # Product event consumer
│   ├── core/
│   │   ├── db_eng.py            # Database engine
│   │   ├── deps.py              # Core dependencies
│   │   ├── requests.py          # HTTP request utilities
│   │   └── utils.py             # Utility functions
│   ├── crud/
│   │   └── inventory_crud.py    # Inventory CRUD operations
│   └── model/
│       └── inventory_model.py   # Database models
├── tests/                       # Unit and integration tests
├── Dockerfile.dev               # Development Docker configuration
├── mypy.ini                     # MyPy configuration
└── pyproject.toml               # Python dependencies
```

## Event Flow

1. **Product Created** → `add_stock` topic → Inventory added
2. **Order Placed** → `update_stock` topic → Stock quantity reduced

## Testing

```bash
pytest tests/
mypy app/
```

## Notes

- Stock updates are processed asynchronously via Kafka consumers
- The service starts consumers on application startup
- Database tables are created automatically
- Authentication required via LoginForAccessToken dependency
