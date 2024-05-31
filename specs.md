## **Overview**

This API documentation outlines the process for non-blocking transactions, specifically for topping up and cashing out using Nil and OSS wallets. The design ensures efficient, secure, and resilient transaction processing, suitable for a growing user base.

## **Table of Contents**

1. [Top-up Process for Nil Users](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#top-up-process-for-nil-users)
    - [Endpoints](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#endpoints)
    - [Web Hooks](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#web-hooks)
    - [Keys Management](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#keys-management)
2. [Cash Out Process for OSS Users](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#cash-out-process-for-oss-users)
    - [Endpoints](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#endpoints-1)
    - [Web Hooks](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#web-hooks-1)
    - [Keys Management](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#keys-management-1)
3. [Error Handling](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#error-handling)
4. [Nil Error Codes](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#nil-error-codes)
5. [Security Considerations](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#security-considerations)
6. [Rate Limiting and Throttling](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#rate-limiting-and-throttling)
7. [Versioning](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#versioning)
8. [Administrative Tasks](https://chatgpt.com/c/4c9c86f8-32f5-4605-8bf5-6e9c6883a045#administrative-tasks)

## **Top-up Process for Nil Users**

### **Endpoints**

### **Initiate Top-up**

- **URL:** **`/api/v1/topup/initiate`**
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
    
    
- **Error Response Example:**
    
    ```json
    
    {
        "status": "error",
        "code": "transaction_not_found",
        "message": "Transaction ID not found.",
        "details": "The specified transaction ID does not exist.",
        "timestamp": "2024-05-24T12:05:00Z",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
    
    ```
    

## **Cash Out Process for OSS Users**

### **Endpoints**

### **Initiate Cash Out**

- **URL:** **`/api/v1/cashout/initiate`**
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

### **Check Transaction Status**

- **URL:** **`/api/v1/transaction/status`**
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
    

### **User Info**

- **URL:** **`/api/v1/user`**
- **Method:** POST
- **Description:** Inquires about a nil user pubic profile
- **Request Body:**
    
    ```json
    {
        "user_id": "string",
        "service_provider": "string",
        "uuid": "string",
        "signed_uuid": "string",
        "timestamp": "string",
    }
    
    ```
    
- **Response:**
    
    ```json
    {
        "status": "success",
        "code": "topup_initiated",
        "message": "Transaction initiated successfully.",
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
        "message": "User doesn't exist ",
        "details": "The user does not have enough balance in their account.",
        "timestamp": "2024-05-24T12:05:00Z",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
    
    ```


### **Web Hooks**

### **Register Web Hook**

To receive updates about transaction status changes, you need to register a web hook URL with us.

**Registration Process:**

1. **URL Registration:**
    - Provide your endpoint URL where you wish to receive updates.
    - Specify the event types you want to subscribe to (e.g., **`transaction_completed`**).

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

1. **On Transaction Completion:**
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

### **Types and Enums**

Documenting various types and enums used in the API:

| Field | Type | Possible Values |
| --- | --- | --- |
| event_type | string | transaction_completed, topup_initiated, cashout_initiated |
| transaction_status | string | pending, failed, success |
| user_status | string | active, inactive, suspended |
| currency | string | USD, EUR, SDG |
| response_status | string | success, error |
| error_code | string | insufficient_balance, transaction_not_found, invalid_uuid, permission_denied, invalid_transaction_id, key_generation_failed, key_rotation_failed, rate_limit_exceeded, bad_request, internal_error |

### **Example Table of Error Codes**

| Error Code | Description | Potential Resolution |
| --- | --- | --- |
| insufficient_balance | Insufficient balance to complete the transaction. | Ensure the user has sufficient funds before retrying the transaction. |
| transaction_not_found | The specified transaction ID does not exist. | Verify the transaction ID and try again. |
| invalid_uuid | The provided UUID does not conform to the required format. | Check the UUID format and correct it before retrying. |
| permission_denied | User does not have permission to perform this operation. | Ensure the user has the necessary permissions before retrying. |
| invalid_transaction_id | The provided transaction ID is invalid. | Check the transaction ID and correct it before retrying. |
| key_generation_failed | An internal server error occurred while attempting to generate keys. | Try again later. If the problem persists, contact support. |
| key_rotation_failed | An internal server error occurred while attempting to rotate keys. | Try again later. If the problem persists, contact support. |
| rate_limit_exceeded | Too Many Requests. | You have exceeded the rate limit. Please try again later. |
| bad_request | Malformed request. Potentially missing some fields. | Refer to API docs. |
| internal_error | A generic error for all server-specific errors. | This indicates a service error at Nil. |

### **Conclusion**

Implementing these suggestions will enhance the clarity, consistency, and usability of your API documentation, providing a better experience for developers integrating with your API.
    

## **Keys Management [INTERNAL]**

Keys management is crucial for secure transaction processing. Nil will generate and issue the private keys, which will then be securely transmitted to the companies. These companies will use the private keys to sign messages, ensuring the integrity and authenticity of transactions.

### **Public/Private Key Management**

### **Generate Keys**

- **URL:** **`/api/v1/keys/generate`**
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

- **URL:** **`/api/v1/keys/rotate`**
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
| bad_request | Malformed request. Potentially missing some fields. | Refer to API docs |
| internal_error | A generic error for all server specific errors | This indicates a service error at nil. |

### Authentication with UUID and Signed UUID

#### Security Model

We use Tailscale as a secure tunnel for server-to-server communication, ensuring that all transactions and data exchanges are encrypted and secure. Tailscale allows us to create a private network, restricting access to only authorized partners. Within this secure network, we further restrict access using public key cryptography. This ensures that each transaction originates exclusively from an authorized account and cannot be tampered with.

#### Implementation of Signed UUIDs

We use RSA keys to sign UUIDs, ensuring that the UUID cannot be tampered with. The signature can be verified using the public key, guaranteeing the authenticity and integrity of the request. Below is a generalized Java implementation for generating a key pair, signing UUIDs, and retrieving the public key.

```java
import java.security.*;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;

public class CryptoUtils {
    private KeyPair keyPair;

    public CryptoUtils() throws Exception {
        initKeyPair();
    }

    private void initKeyPair() throws Exception {
        KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
        keyPairGenerator.initialize(2048);
        keyPair = keyPairGenerator.generateKeyPair(); // you will be retrieving this private key from your *own* secure storage
    }

    public String signUUID(String uuid) throws Exception {
        Signature signature = Signature.getInstance("SHA256withRSA");
        signature.initSign(keyPair.getPrivate());
        signature.update(uuid.getBytes());
        byte[] signedData = signature.sign();
        return Base64.getEncoder().encodeToString(signedData);
    }

    public String getPublicKey() throws Exception {
        PublicKey publicKey = keyPair.getPublic();
        X509EncodedKeySpec x509EncodedKeySpec = new X509EncodedKeySpec(publicKey.getEncoded());
        return Base64.getEncoder().encodeToString(x509EncodedKeySpec.getEncoded());
    }

    public static void main(String[] args) {
        try {
            CryptoUtils cryptoUtils = new CryptoUtils();
            String uuid = "your-uuid-here";
            String signedUUID = cryptoUtils.signUUID(uuid);
            System.out.println("Signed UUID: " + signedUUID);
            System.out.println("Public Key: " + cryptoUtils.getPublicKey());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### Explanation

1. **Key Pair Generation:**
   - The `CryptoUtils` class generates an RSA key pair upon instantiation.
   - The `initKeyPair` method initializes the RSA key pair with a key size of 2048 bits.

2. **Signing UUIDs:**
   - The `signUUID` method takes a UUID string, signs it using the private key, and returns the Base64-encoded signed data.
   - The signature algorithm used is `SHA256withRSA`.

3. **Retrieving the Public Key:**
   - The `getPublicKey` method returns the Base64-encoded public key in the X.509 format.
   - This public key can be distributed to clients or other servers to verify the signed UUIDs.

4. **Usage Example:**
   - The `main` method demonstrates how to use the `CryptoUtils` class to sign a UUID and retrieve the public key.
   - Replace `"your-uuid-here"` with the actual UUID to be signed.

#### Ensuring Secure Transactions

By combining Tailscale for secure tunneling with RSA-signed UUIDs, we achieve a multi-layered security approach:
- **Tailscale Network:** Restricts access to only authorized partners, creating a secure and private network environment.
- **Public Key Cryptography:** Ensures that each transaction is authenticated and its integrity is verified, allowing only authorized accounts to initiate transactions within the secure network.

This guarantees robust security for the API, ensuring that all requests are both encrypted and authenticated, thereby preventing unauthorized access and ensuring the authenticity of each transaction.

## **Security Considerations**

- **VPN Tunnels:** Secure VPN tunnels (Tailscale) for server-to-server communication.
- **Exponential Backoff:** Retry connections with exponential backoff, up to a specified number of attempts.
- **Reversal Mechanism:** Failed transactions moved to a reversal account for dedicated processing.
- **Audit Logs:** All transactions and key management operations logged for auditing.
- **Encryption:** Sensitive data encrypted in transit and at rest.
- **Authentication and Authorization:** Robust mechanisms to control API access.

## **Rate Limiting and Throttling**

- **Policy:**
    - Maximum 100 requests per minute per user.
    - Requests exceeding the limit receive a **`429 Too Many Requests`** response.
- **Response for Exceeded Rate Limit:**
    
    ```json
    
    {
        "status": "error",
        "code": "rate_limit_exceeded",
        "message": "Too Many Requests",
        "details": "You have exceeded the rate limit. Please try again later.",
        "retry_after": "60",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
    
    ```
    

## **Versioning**

- **Current Version:** v1
- **Versioning Strategy:**
    - Major versions indicate breaking changes.
    - Minor versions indicate backward-compatible additions.
    - Patch versions indicate backward-compatible bug fixes.
- **Versioning Example:**
    - **URL:** **`/api/v1/topup/initiate`**
    - **Version:** v1

## **Administrative Tasks [INTERNAL]**

### **Tailscale VPN Integration**

For secure communication between Nil and service providers, Tailscale VPN is used. If hosting systems on AWS or other cloud providers, Tailscale can be integrated into their environments.

### **Tailscale with AWS VPC**

To integrate Tailscale with AWS VPC:

1. **Install Tailscale on EC2 Instances:**
    - SSH into your EC2 instance.
    - Install Tailscale by following the installation instructions for your OS from the [Tailscale documentation](https://tailscale.com/kb/1296/aws-reference-architecture).
2. **Authenticate Tailscale:**
    - Run **`tailscale up`** and authenticate using your Tailscale account.
3. **Configure VPC Peering:**
    - Use Tailscale's subnet routing feature to route traffic from the VPC to Tailscale nodes. This involves setting up Tailscale on a subnet router within your VPC.
4. **Setup Subnet Routes:**
    - Enable subnet routes on Tailscale by running **`tailscale up --advertise-routes=10.0.0.0/16`** (replace with your VPC CIDR).

### **Tailscale with Other Cloud Providers**

For other cloud providers like GCP, Azure, or DigitalOcean, the process is similar. Install Tailscale on your virtual machines, authenticate, and configure subnet routing as per the provider's network setup.

### **Accessing Connected Tailscale Nodes**

To manage and access information about connected Tailscale nodes:

1. **Tailscale Admin Console:**
    - Use the Tailscale admin console to view all connected nodes, their IP addresses, and status.
2. **Tailscale API:**
    - Use the Tailscale API to programmatically access information about nodes, including their connection status and details.

### **Tailscale ACL Configuration**

To limit service provider interaction to a subset of endpoints, configure Tailscale ACLs:

```json
{
  "tagOwners": {
    "tag:service-providers": ["<identity-of-node-allowed-to-tag>"]
  },
  "acl": [
    {
      "action": "accept",
      "src": ["tag:service-providers"],
      "dst": ["10.0.0.0/24:*"]
    }
  ]
}

```

### **Steps to Apply ACL**

1. **Access Tailscale Admin Console:**
    - Go to the Tailscale admin console.
    - Navigate to the "Access Controls" section.
2. **Edit ACLs:**
    - Add the ACL JSON configuration.
    - Save and apply the changes.

## **Data Field Rationale**

In our API responses, the **`data`** field will contain different information depending on the context of the API call. Here is a breakdown of the possible structures:

### **Top-up Response Data**

- **Fields:**
    - **`transaction_id`**: Unique identifier for the transaction.
    - **`status`**: Status of the transaction (e.g., pending, completed).
    - **`amount`**: Amount involved in the transaction.
    - **`currency`**: Currency of the transaction.
    - **`uuid`**: Unique identifier for the request.
    - **`signed_uuid`**: Signed unique identifier for the request.
- **Example:**
    
    ```json
    
    {
        "transaction_id": "txn_001",
        "status": "pending",
        "amount": 100.00,
        "currency": "USD",
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001"
    }
    
    ```
    

### **Cash-out Response Data**

- **Fields:**
    - **`transaction_id`**: Unique identifier for the transaction.
    - **`status`**: Status of the transaction (e.g., pending, completed).
    - `amount**: Amount involved in the transaction.**`
    - currency**`: Currency of the transaction.`**
    - **ti**mestamp**`: Timestamp of the transaction.`**
    - uuid**`: Unique identifier for the request.`**
    - signed_uuid: Signed unique identifier for the request.
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
    

### **General API Response Data**

- **Description:** This field will be used to return specific data relevant to the context of the API call, ensuring that responses are informative and useful for the API consumers.
- **Note:** Always refer to the specific API endpoint documentation to understand the exact structure and contents of the **`data`** field for that endpoint.

## **Feature Implementation Timeline**

[Service Provider Integration](https://www.notion.so/d838393925774bfcbe92adbb41d93091?pvs=21)

## Extra Features

- Check balance (for OSS accounts)
- Check statement
- Create sequence diagrams

This updated documentation provides a comprehensive, consistent, and clear structure for API responses, enhancing the user experience for API consumers.