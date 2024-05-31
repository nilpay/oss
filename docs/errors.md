## **Error Handling**

### **Standard Error Response:**

```json
{
    "status": "error",
    "code": "string",
    "message": "string",
    "details": "string",
    "timestamp": "string",
    "uuid": "string",
    "signed_uuid": "string"
}
```

### **Common Error Codes:**

- **insufficient_balance**
    - Message: "Insufficient balance to complete the transaction."
    - Details: "The user does not have enough balance in their account."
    - Resolution: "Ensure the user has sufficient funds before retrying the transaction."
- **transaction_not_found**
    - Message: "Transaction ID not found."
    - Details: "The specified transaction ID does not exist."
    - Resolution: "Verify the transaction ID and try again."
- **invalid_uuid**
    - Message: "Invalid UUID format."
    - Details: "The provided UUID does not conform to the required format."
    - Resolution: "Check the UUID format and correct it before retrying."
- **permission_denied**
    - Message: "User does not have permission to perform this operation."
    - Details: "The authenticated user is not authorized to perform this operation."
    - Resolution: "Ensure the user has the necessary permissions before retrying."
- **invalid_transaction_id**
    - Message: "Invalid transaction ID."
    - Details: "The provided transaction ID is invalid."
    - Resolution: "Check the transaction ID and correct it before retrying."
- **key_generation_failed**
    - Message: "Unable to generate keys due to server error."
    - Details: "An internal server error occurred while attempting to generate keys."
    - Resolution: "Try again later. If the problem persists, contact support."
- **key_rotation_failed**
    - Message: "Unable to rotate keys due to server error."
    - Details: "An internal server error occurred while attempting to rotate keys."
    - Resolution: "Try again later. If the problem persists, contact support."

## **Nil Error Codes**

| Error Code | Description | Potential Resolution |
| --- | --- | --- |
| insufficient_balance | Insufficient balance to complete the transaction. | Ensure the user has sufficient funds before retrying the transaction. |
| transaction_not_found | The specified transaction ID does not exist. | Verify the transaction ID and try again. |
| invalid_uuid | The provided UUID does not conform to the required format. | Check the UUID format and correct it before retrying. |
| permission_denied | The authenticated user is not authorized to perform this operation. | Ensure the user has the necessary permissions before retrying. |
| invalid_transaction_id | The provided transaction ID is invalid. | Check the transaction ID and correct it before retrying. |
| key_generation_failed | An internal server error occurred while attempting to generate keys. | Try again later. If the problem persists, contact support. |
| key_rotation_failed | An internal server error occurred while attempting to rotate keys. | Try again later. If the problem persists, contact support. |
| bad_request | Malformed request. Potentially missing some fields. | Refer to API docs. |
| internal_error | A generic error for all server-specific errors. | This indicates a service error at Nil. |