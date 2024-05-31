## **Keys Management [INTERNAL]**

### **Generate Keys**

- **URL:** `/api/v1/keys/generate`
- **Method:** POST
- **Description:** Generates a new pair of public and private keys.
- **Response:**
    
```json
{
    "status": "success",
    "code": "keys_generated",
    "message": "Keys generated successfully.",
    "data": {
        "public_key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7...",
        "private_key": "MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAo...",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
}
```
    
- **Error Response Example:**
    
```json
{
    "status": "error",
    "code": "key_generation_failed",
    "message": "Unable to generate keys due to server error.",
    "details": "An internal server error occurred while attempting to generate keys.",
    "timestamp": "2024-05-24T12:10:00Z",
    "uuid": "uuid_001",
    "signed_uuid": "signed_uuid_001"
}
```

### **Rotate Keys**

- **URL:** `/api/v1/keys/rotate`
- **Method:** POST
- **Description:** Rotates the existing keys and generates a new pair of keys.
- **Response:**
    
```json
{
    "status": "success",
    "code": "keys_rotated",
    "message": "Keys rotated successfully.",
    "data": {
        "new_public_key": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7...",
        "new_private_key": "MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAo...",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
}
```
    
- **Error Response Example:**
    
```json
{
    "status": "error",
    "code": "key_rotation_failed",
    "message": "Unable to rotate keys due to server error.",
    "details": "An internal server error occurred while attempting to rotate keys.",
    "timestamp": "2024-05-24T12:10:00Z",
    "uuid": "uuid_001",
    "signed_uuid": "signed_uuid_001"
}
```