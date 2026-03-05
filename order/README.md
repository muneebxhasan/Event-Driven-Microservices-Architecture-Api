# Order Service

## Overview

The Order Service handles all order-related operations including order creation, management, and payment integration. It coordinates with Payment, Inventory, and Notification services.

## Features

- Order creation and management
- Payment processing integration (Stripe)
- Order status tracking
- Event-driven order processing
- Payment status updates via Kafka
- Stock validation using gRPC

## Technology Stack

- **Framework**: FastAPI
- **Database**: PostgreSQL
- **Message Broker**: Apache Kafka
- **ORM**: SQLModel
- **Serialization**: Protocol Buffers (Protobuf)
- **Payment Gateway**: Stripe

## Port Configuration

- **Service Port**: 8087
- **Database Port**: 5435

## API Endpoints

- **Root Path**: `/order-service`
- **Development Server**: `http://127.0.0.1:8087`
- **API Documentation**: `http://127.0.0.1:8087/docs`

## Database Configuration

- **Database Name**: order_postgres_db
- **Container Name**: OrderCont
- **User**: muneeb
- **Host**: localhost:5435

## Kafka Topics

The service interacts with the following topics:

- **payment**: Consumes payment status updates
- **update_stock**: Publishes stock update requests
- **order_conformation_notification**: Publishes order confirmation events

## Running the Service

### Using Docker Compose

```bash
docker-compose up order_service
```

### Local Development

```bash
cd order
pip install -e .
uvicorn app.main:app --reload --port 8087
```

## Project Structure

```
order/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI application entry point
│   ├── setting.py           # Configuration settings
│   ├── stock_pb2.py         # Protobuf generated code
│   ├── stock.proto          # Protobuf definitions
│   ├── api/
│   │   ├── dep.py           # API dependencies
│   │   └── v1/              # API v1 routes
│   ├── consumer/
│   │   └── payment.py       # Payment event consumer
│   ├── core/
│   │   ├── db_eng.py        # Database engine
│   │   ├── dep_kafka.py     # Kafka dependencies
│   │   ├── requests.py      # HTTP request utilities
│   │   └── utils.py         # Utility functions
│   ├── crud/
│   │   └── order_crud.py    # Order CRUD operations
│   └── models/
│       └── order_model.py   # Database models
├── tests/                   # Unit and integration tests
├── Dockerfile.dev           # Development Docker configuration
└── pyproject.toml           # Python dependencies
```

## Order Flow

1. **User places order** → Order created with PENDING status
2. **Payment initiated** → Redirect to Stripe checkout
3. **Payment completed** → Kafka event consumed → Order status updated
4. **Stock updated** → Event published to inventory service
5. **Notification sent** → Event published to notification service

## Payment Integration

The service integrates with Stripe for payment processing:

- Creates checkout sessions
- Handles payment callbacks
- Updates order status based on payment events

## Testing

```bash
pytest tests/
```

## Notes

- Requires authentication via LoginForAccessToken dependency
- CORS enabled for all origins (development mode)
- Automatic database table creation on startup
- Payment consumer runs as background task
