## **Cash Out Process for OSS Users**

### **Endpoints**

### **Initiate Cash Out**

- **URL:** `/api/v1/cashout/initiate`
- **Method:** POST
- **Description:** Initiates a cash-out transaction for an OSS user.
- **Request Body:**
    
```json
{
    "uuid": "string",
    "signed_uuid": "string",
    "timestamp": "string",
    "from_account": "string",
    "to_account": "string",
    "amount": "number",
    "currency": "string",
    "notes": "string",
    "service_provider": "string"
}
```
    
- **Response:**

```json
{
    "status": "success",
    "code": "cashout_initiated",
    "message": "Cash out request processed successfully.",
    "data": {
        "transaction_id": "txn_002",
        "status": "pending",
        "amount": 200.00,
        "currency": "USD",
        "timestamp": "2024-05-24T12:10:00Z",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
}
```
    
- **Error Response Example:**
    
```json
{
    "status": "error",
    "code": "permission_denied",
    "message": "User does not have permission to perform this operation.",
    "details": "The authenticated user is not authorized to perform this cash-out operation.",
    "timestamp": "2024-05-24T12:10:00Z",
    "uuid": "uuid_001",
    "signed_uuid": "signed_uuid_001"
}
```