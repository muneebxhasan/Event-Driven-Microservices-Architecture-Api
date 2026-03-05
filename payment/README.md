# Payment Service

## Overview

The Payment Service integrates Stripe payment gateway for processing payments in the e-commerce platform. It handles checkout sessions, payment confirmations, and communicates payment status to other services.

## Features

- Stripe checkout integration
- Payment session creation
- Payment confirmation handling
- Event publishing to Kafka for order updates
- Success/Cancel page handling
- Static file serving for payment UI

## Technology Stack

- **Framework**: FastAPI
- **Payment Gateway**: Stripe
- **Message Broker**: Apache Kafka
- **Template Engine**: Jinja2
- **Database**: PostgreSQL

## Port Configuration

- **Service Port**: 8090
- **Database Port**: 5438

## API Endpoints

- **Root Path**: `/payment-service`
- **Development Server**: `http://127.0.0.1:8090`
- **Payment Checkout**: `http://127.0.0.1:8090/payment`
- **Success Callback**: `http://127.0.0.1:8090/success`
- **Cancel Callback**: `http://127.0.0.1:8090/cancel`

## Database Configuration

- **Database Name**: payment_postgres_db
- **Container Name**: PaymentCont
- **User**: muneeb
- **Host**: localhost:5438

## Environment Variables

Configure the following:

- `STRIPE_SECRET_KEY`: Stripe API secret key
- `YOUR_DOMAIN`: Application domain for callbacks
- Kafka bootstrap servers

## Kafka Topics

The service publishes to:

- **payment**: Payment status updates
- **order_conformation_notification**: Order confirmation events

## Running the Service

### Using Docker Compose

```bash
docker-compose up payment_service
```

### Local Development

```bash
cd payment
pip install -e .
uvicorn app.main:app --reload --port 8090
```

## Project Structure

```
payment/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI application entry point
│   ├── setting.py           # Configuration settings
│   └── core/
│       └── dep_kafka.py     # Kafka producer dependency
├── public/                  # Static files
│   ├── checkout.html        # Checkout page
│   ├── success.html         # Payment success page
│   ├── cancel.html          # Payment cancelled page
│   └── style.css            # Styling
├── tests/                   # Unit and integration tests
├── Dockerfile.dev           # Development Docker configuration
└── pyproject.toml           # Python dependencies
```

## Payment Flow

1. **Create Checkout** → Generate Stripe session with order metadata
2. **Redirect to Stripe** → User completes payment on Stripe
3. **Payment Success** → Stripe redirects to `/success` endpoint
4. **Retrieve Session** → Get order details from Stripe session
5. **Publish Events** → Send payment confirmation to Kafka
6. **Update Order** → Order service receives event and updates status

## Stripe Integration

The service uses Stripe Checkout:

- Creates checkout sessions with order and user metadata
- Handles success/cancel redirects
- Retrieves session details for confirmation
- Supports various payment methods

## Static Files

The service serves static HTML/CSS files from the `public` directory:

- **checkout.html**: Payment initiation page
- **success.html**: Payment confirmation page
- **cancel.html**: Payment cancellation page

## Event Publishing

After successful payment:

1. Publishes to `payment` topic with order_id and user_id
2. Publishes to `order_conformation_notification` for notifications

## Testing

```bash
pytest tests/
```

## Stripe Webhook (Future Enhancement)

For production, consider implementing Stripe webhooks for:

- Payment intent success/failure
- Refund handling
- Dispute management

## Notes

- Requires valid Stripe API keys
- Static files mounted at `/app` route
- Session metadata includes order_id and user_id
- Integrated with Kong API Gateway
