# Notification Service

## Overview

The Notification Service handles all notification-related operations including email notifications for user registration, order confirmations, and other system events.

## Features

- Email notification system
- User registration notifications
- Order confirmation notifications
- Event-driven notification processing
- Asynchronous email sending
- Multiple Kafka consumer integration

## Technology Stack

- **Framework**: FastAPI
- **Database**: PostgreSQL
- **Message Broker**: Apache Kafka
- **ORM**: SQLModel
- **Email**: SMTP integration

## Port Configuration

- **Service Port**: 8089
- **Database Port**: 5437

## API Endpoints

- **Root Path**: `/notification-service`
- **Development Server**: `http://127.0.0.1:8089`
- **API Documentation**: `http://127.0.0.1:8089/docs`

## Database Configuration

- **Database Name**: notification_postgres_db
- **Container Name**: notificationCont
- **User**: muneeb
- **Host**: localhost:5437

## Kafka Topics

The service consumes from:

- **user_notify**: User registration events
- **order_conformation_notification**: Order confirmation events
- **order_notification**: General order notifications

## Running the Service

### Using Docker Compose

```bash
docker-compose up notification_service
```

### Local Development

```bash
cd notification
pip install -e .
uvicorn app.main:app --reload --port 8089
```

## Project Structure

```
notification/
├── app/
│   ├── __init__.py
│   ├── main.py                    # FastAPI application entry point
│   ├── setting.py                 # Configuration settings
│   ├── model.py                   # Database models
│   ├── crud.py                    # CRUD operations
│   ├── send_mail.py               # Email sending logic
│   ├── send_notification.py       # Notification orchestration
│   ├── consumer/
│   │   ├── get_user.py            # User data consumer
│   │   ├── order_consumer.py      # Order event consumer
│   │   ├── user_consumer.py       # User detail consumer
│   │   └── user_register.py       # Registration event consumer
│   └── core/
│       ├── db_eng.py              # Database engine
│       ├── dep_kafka.py           # Kafka dependencies
│       └── requests.py            # HTTP request utilities
├── tests/                         # Unit and integration tests
├── Dockerfile.dev                 # Development Docker configuration
└── pyproject.toml                 # Python dependencies
```

## Notification Flow

1. **Event Published** → Kafka topic
2. **Consumer Receives** → Parse event data
3. **Get User Details** → Fetch recipient information
4. **Send Email** → Via SMTP server
5. **Log Result** → Store in database

## Email Configuration

Configure the following environment variables:

- `SMTP_HOST`: SMTP server host
- `SMTP_PORT`: SMTP server port
- `SMTP_USER`: Email account username
- `SMTP_PASSWORD`: Email account password
- `FROM_EMAIL`: Sender email address

## Supported Notifications

- **User Registration**: Welcome email to new users
- **Order Confirmation**: Order details and receipt
- **Payment Confirmation**: Payment success notification
- **General Notifications**: System alerts and updates

## Testing

```bash
pytest tests/
```

## API Endpoints

- `GET /`: Service health check
- `GET /send-email/asynchronous`: Send test email asynchronously

## Notes

- Multiple Kafka consumers run as background tasks
- CORS enabled for all origins (development mode)
- Database tables created automatically on startup
- Asynchronous email sending for better performance
