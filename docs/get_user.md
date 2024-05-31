### **User Info**

- **URL:** `/api/v1/user`
- **Method:** POST
- **Description:** Inquires about a nil user public profile
- **Request Body:**
    
```json
{
    "user_id": "string",
    "service_provider": "string",
    "uuid": "string",
    "signed_uuid": "string",
    "timestamp": "string"
}
```
    
- **Response:**
    
```json
{
    "status": "success",
    "code": "user_info_retrieved",
    "message": "User info retrieved successfully.",
    "data": {
        "user_id": "user_1",
        "fullname": "Mohamed Ahmed",
        "status": "active"
    }
}
```
    
- **Error Response Example:**
    
```json
{
    "status": "error",
    "code": "invalid_user_id",
    "message": "User doesn't exist.",
    "details": "The user ID provided is invalid.",
    "timestamp": "2024-05-24T12:05:00Z",
    "uuid": "uuid_001",
    "signed_uuid": "signed_uuid_001"
}
```