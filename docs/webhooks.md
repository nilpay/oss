## **Web Hooks**

### **Register Web Hook**

To receive updates about transaction status changes, you need to register a web hook URL with us.

**Registration Process:**

1. **URL Registration:**
    - Provide your endpoint URL where you wish to receive updates.
    - Specify the event types you want to subscribe to (e.g., `transaction_completed`).

**Request Example:**

```json
{
    "endpoint_url": "https://example.com/webhook",
    "event_type": "transaction_completed",
    "service_provider": "string",
    "uuid": "string",
    "signed_uuid": "string"
}
```

**Response Example:**

```json
{
    "status": "success",
    "code": "webhook_registered",
    "message": "Web hook registered successfully.",
    "data": {
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001",
        "service_provider": "249_111"
    }
}
```

2. **On Transaction Completion:**
    - When a transaction is completed, we will send a POST request to your registered URL with the following fields:

**Request Body Example:**

```json
{
    "transaction_id": "string",
    "status": "string",
    "message": "string",
    "updated_at": "string",
    "uuid": "string",
    "signed_uuid": "string"
}
```

**Response Example:**

```json
{
    "status": "success",
    "code": "webhook_received",
    "message": "Webhook received successfully.",
    "data": {
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
}
```