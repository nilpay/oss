
### **Initiate Top-up**

- **URL:** `/api/v1/topup/initiate`
- **Method:** POST
- **Description:** Initiates a top-up transaction for a Nil user.
- **Request Body:**
    
```json
{
    "user_id": "string",
    "amount": "number",
    "service_provider": "string",
    "uuid": "string",
    "signed_uuid": "string",
    "timestamp": "string",
    "currency": "string"
}
```
    
- **Response:**
    
```json
{
    "status": "success",
    "code": "topup_initiated",
    "message": "Transaction initiated successfully.",
    "data": {
        "transaction_id": "txn_001",
        "status": "pending",
        "amount": 100.00,
        "currency": "USD",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
}
```
    
- **Error Response Example:**
    
```json
{
    "status": "error",
    "code": "insufficient_balance",
    "message": "Insufficient balance to complete the transaction.",
    "details": "The user does not have enough balance in their account.",
    "timestamp": "2024-05-24T12:05:00Z",
    "uuid": "uuid_001",
    "signed_uuid": "signed_uuid_001"
}
```