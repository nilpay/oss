### **Check Transaction Status**

- **URL:** `/api/v1/transaction/status`
- **Method:** POST
- **Description:** Checks the status of a transaction.
- **Request Parameters:**
    
```json
{
    "uuid": "string",
    "signed_uuid": "string",
    "timestamp": "string",
    "service_provider": "string",
    "original_uuid": "string"
}
```
    
- **Response:**
    
```json
{
    "status": "success",
    "code": "transaction_status_retrieved",
    "message": "Transaction status retrieved successfully.",
    "data": {
        "original_uuid": "txn_001",
        "status": "completed",
        "type": "top-up",
        "details": "Transaction processed successfully.",
        "last_checked": "2024-05-24T12:05:00Z",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
}
```