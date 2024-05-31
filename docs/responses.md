### **General API Response Data**

- **Description:** This field will be used to return specific data relevant to the context of the API call, ensuring that responses are informative and useful for the API consumers.
- **Note:** Always refer to the specific API endpoint documentation to understand the exact structure and contents of the `data` field for that endpoint.

### **Cash-out Response Data**

- **Fields:**
    - `transaction_id`: Unique identifier for the transaction.
    - `status`: Status of the transaction (e.g., pending, completed).
    - `amount`: Amount involved in the transaction.
    - `currency`: Currency of the transaction.
    - `timestamp`: Timestamp of the transaction.
    - `uuid`: Unique identifier for the request.
    - `signed_uuid`: Signed unique identifier for the request.
- **Example:**
    
```json
{
    "transaction_id": "txn_002",
    "status": "pending",
    "amount": 200.00,
    "currency": "USD",
    "timestamp": "2024-05-24T12:10:00Z",
    "uuid": "uuid_001",
    "signed_uuid": "signed_uuid_001"
}
```


