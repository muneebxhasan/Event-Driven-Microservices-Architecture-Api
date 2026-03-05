# MartAPI - E-Commerce Microservices Platform

## Overview

MartAPI is a comprehensive e-commerce platform built with microservices architecture using FastAPI, PostgreSQL, Apache Kafka, and Kong API Gateway. The platform consists of six independent services that communicate via REST APIs and event-driven messaging.

## Architecture

The platform follows a microservices architecture with:

- **API Gateway**: Kong for routing and authentication
- **Message Broker**: Apache Kafka for event-driven communication
- **Databases**: PostgreSQL for each service (database per service pattern)
- **Service Discovery**: Docker Compose networking
- **Tunnel**: Cloudflared for external access

## Services

### 1. Product Service (Port 8085)

Manages product catalog and inventory items.

- Product CRUD operations
- Product search and filtering
- Protobuf for efficient communication

### 2. Inventory Service (Port 8086)

Tracks stock levels and inventory management.

- Real-time stock updates
- Consumes product and order events
- Automatic stock reduction on orders

### 3. Order Service (Port 8087)

Handles order processing and management.

- Order creation and tracking
- Payment integration
- Order status management

### 4. User Service (Port 8088)

Manages user authentication and profiles.

- JWT-based authentication
- User registration and login
- Role-based access control

### 5. Notification Service (Port 8089)

Sends notifications and emails.

- Email notifications
- Order confirmations
- User registration emails

### 6. Payment Service (Port 8090)

Processes payments via Stripe.

- Stripe checkout integration
- Payment confirmations
- Refund handling

## Technology Stack

### Backend

- **Framework**: FastAPI
- **ORM**: SQLModel
- **Database**: PostgreSQL
- **Message Broker**: Apache Kafka 3.7.0
- **API Gateway**: Kong
- **Payment**: Stripe

### DevOps

- **Containerization**: Docker & Docker Compose
- **Tunneling**: Cloudflared
- **Monitoring**: Kafka UI

## Prerequisites

- Docker and Docker Compose
- Python 3.11+
- Stripe account (for payment processing)

## Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
cd martapi
```

### 2. Configure Environment

Create `.env` file or update settings in each service.

### 3. Start All Services

```bash
docker-compose up --build
```

### 4. Access Services

- **Product Service**: http://localhost:8085/docs
- **Inventory Service**: http://localhost:8086/docs
- **Order Service**: http://localhost:8087/docs
- **User Service**: http://localhost:8088/docs
- **Notification Service**: http://localhost:8089/docs
- **Payment Service**: http://localhost:8090/payment
- **Kafka UI**: http://localhost:8080
- **Kong Admin**: http://localhost:8001
- **Kong Proxy**: http://localhost:8000

## Database Ports

- Product DB: 5433
- Inventory DB: 5434
- Order DB: 5435
- User DB: 5436
- Notification DB: 5437
- Payment DB: 5438
- Kong DB: (internal)

## Kafka Topics

- `add_stock`: Product addition events
- `update_stock`: Stock level updates
- `payment`: Payment status events
- `order_conformation_notification`: Order confirmations
- `user_notify`: User registration events
- `get_user`: User detail requests
- `order_notification`: General order notifications

## Event Flow

### Order Creation Flow

1. User creates order → **Order Service**
2. Order service creates checkout → **Payment Service**
3. Payment confirmed → Kafka `payment` topic
4. Order service updates order status
5. Stock reduced → Kafka `update_stock` topic → **Inventory Service**
6. Notification sent → Kafka `order_conformation_notification` → **Notification Service**

### Product Creation Flow

1. Product created → **Product Service**
2. Stock added → Kafka `add_stock` topic → **Inventory Service**

## API Gateway (Kong)

Kong is configured as the API gateway with:

- Route management
- Authentication
- Rate limiting
- Load balancing

Access services through Kong proxy:

```
http://localhost:8000/product-service/*
http://localhost:8000/inventory-service/*
http://localhost:8000/order-service/*
http://localhost:8000/user-service/*
http://localhost:8000/notification-service/*
http://localhost:8000/payment-service/*
```

## Development

### Running Individual Services

```bash
# Product Service
docker-compose up product_service

# Inventory Service
docker-compose up inventory_service

# Order Service
docker-compose up order_service

# User Service
docker-compose up user_service

# Notification Service
docker-compose up notification_service

# Payment Service
docker-compose up payment_service
```

### Viewing Logs

```bash
docker-compose logs -f <service_name>
```

### Accessing Service Shell

```bash
docker exec -it <container_name> /bin/bash
```

## Testing

Each service has its own test suite:

```bash
# Run tests for a specific service
cd <service_directory>
pytest tests/
```

## Project Structure

```
martapi/
├── compose.yaml                 # Docker Compose configuration
├── readme.md                    # This file
├── POSTGRES_PASSWORD            # Kong DB password
├── gateway-startup.sh           # Gateway initialization script
├── cloudflared/                 # Cloudflare tunnel config
├── product/                     # Product Service
├── inventory/                   # Inventory Service
├── order/                       # Order Service
├── UserService/                 # User Service
├── notification/                # Notification Service
└── payment/                     # Payment Service
```

## Configuration

### Database Credentials

Default credentials (change in production):

- **User**: muneeb / ziakhan
- **Password**: my_password

### Kafka Configuration

- **Broker**: localhost:9092
- **Internal Broker**: broker:19092
- **Cluster ID**: 4L6g3nShT-eMCtK--X86sw

## Monitoring

- **Kafka UI**: Monitor topics, messages, and consumer groups at http://localhost:8080

## Security Considerations

- Change default database passwords
- Use environment variables for sensitive data
- Implement proper authentication in Kong
- Enable HTTPS in production
- Secure Kafka with SASL/SSL
- Validate JWT tokens properly

## Troubleshooting

### Service won't start

```bash
docker-compose down -v
docker-compose up --build
```

### Database connection issues

Check if database containers are running:

```bash
docker ps | grep Cont
```

### Kafka consumer not receiving messages

Check Kafka UI at http://localhost:8080 for topic status

## Production Deployment

For production deployment:

1. Use proper secrets management
2. Enable Kong authentication plugins
3. Set up SSL/TLS certificates
4. Configure Kafka replication
5. Implement database backups
6. Set up monitoring and logging
7. Use production-grade SMTP server
8. Configure Stripe webhooks

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

[Add your license here]

## Support

For issues and questions, please open an issue in the repository.
