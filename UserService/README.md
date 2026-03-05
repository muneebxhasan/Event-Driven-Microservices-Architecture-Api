# User Service

## Overview

The User Service manages user authentication, authorization, and user profile operations. It provides JWT-based authentication and integrates with other services via Kafka.

## Features

- User registration and authentication
- JWT token-based authorization
- User profile management
- Role-based access control
- Event publishing for user-related actions
- Integration with Notification service

## Technology Stack

- **Framework**: FastAPI
- **Database**: PostgreSQL
- **Message Broker**: Apache Kafka
- **ORM**: SQLModel
- **Authentication**: JWT (JSON Web Tokens)

## Port Configuration

- **Service Port**: 8088
- **Database Port**: 5436

## API Endpoints

- **Root Path**: `/user-service`
- **Development Server**: `http://127.0.0.1:8088`
- **API Documentation**: `http://127.0.0.1:8088/docs`

## Database Configuration

- **Database Name**: user_auth_postgres_db
- **Container Name**: UserCont
- **User**: muneeb
- **Host**: localhost:5436

## Kafka Topics

The service publishes to:

- **get_user**: User detail events
- **user_notify**: User registration notifications

## Running the Service

### Using Docker Compose

```bash
docker-compose up user_service
```

### Local Development

```bash
cd UserService
pip install -e .
uvicorn app.main:app --reload --port 8088
```

## Project Structure

```
UserService/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI application entry point
│   ├── setting.py           # Configuration settings
│   ├── utiles.py            # Utility functions
│   ├── initial_data.py      # Database initialization
│   ├── api/
│   │   └── v1/              # API v1 routes
│   ├── consumer/
│   │   └── notification_consumer.py  # Kafka consumers
│   ├── core/
│   │   └── db_eng.py        # Database engine
│   ├── crud/                # CRUD operations
│   └── model/               # Database models
├── tests/                   # Unit and integration tests
├── Dockerfile.dev           # Development Docker configuration
└── pyproject.toml           # Python dependencies
```

## Authentication Flow

1. **User Registration** → Create user account → Publish to Kafka
2. **User Login** → Validate credentials → Return JWT token
3. **Protected Routes** → Verify JWT token → Allow access

## Initial Data

The service includes an `initial_data.py` script that can be used to:

- Create default admin user
- Set up initial roles and permissions
- Populate sample data for development

## Security Features

- Password hashing
- JWT token generation and validation
- Token expiration handling
- Secure password storage

## Testing

```bash
pytest tests/
```

## Notes

- CORS enabled for all origins (development mode)
- Database tables created automatically on startup
- Initial data populated during application lifespan
- Consumer for user details starts on application startup
