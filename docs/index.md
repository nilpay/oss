## **API Documentation**

## **Overview**

This API documentation outlines the process for non-blocking transactions, specifically for topping up and cashing out using Nil and OSS wallets. The design ensures efficient, secure, and resilient transaction processing, suitable for a growing user base.

## **Table of Contents**

1. [Top-up Process for Nil Users](#top-up-process-for-nil-users)
    - [Endpoints](#endpoints)
    - [Web Hooks](#web-hooks)
    - [Keys Management](#keys-management)
2. [Cash Out Process for OSS Users](#cash-out-process-for-oss-users)
    - [Endpoints](#endpoints-1)
    - [Web Hooks](#web-hooks-1)
    - [Keys Management](#keys-management-1)
3. [Error Handling](#error-handling)
4. [Nil Error Codes](#nil-error-codes)
5. [Security Considerations](#security-considerations)
6. [Rate Limiting and Throttling](#rate-limiting-and-throttling)
7. [Versioning](#versioning)
8. [Administrative Tasks](#administrative-tasks)

## **Top-up Process for Nil Users**

### **Endpoints**

!!! tip
    Base url for accessing our endpoint behind tailscale VPN is: https://nil.tailf4a43a.ts.net

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
| authentication_error | An error in nil authentication. Are you following our encryption tutorial? | Ensure you are signing the correct UUID |


Certainly! Here is a more detailed section focusing on the status and code values for the API responses, following best practices and ensuring clarity:

### Status and Code Usage

- **`status`**: Indicates the overall state of the transaction.
  - Possible values: `pending`, `failed`, `success`
- **`code`**: Provides more specific information about the outcome of the transaction.
  - Possible values include specific success or error codes.

### Detailed Tables for Status and Code

#### Status

| Status    | Description                                    |
|-----------|------------------------------------------------|
| pending   | The transaction is in progress and not yet complete. |
| failed    | The transaction has failed due to an error.    |
| success   | The transaction has completed successfully.    |

#### Code

##### Success Codes

| Code                     | Description                                             |
|--------------------------|---------------------------------------------------------|
| topup_initiated          | The top-up transaction has been initiated successfully. |
| cashout_initiated        | The cash-out transaction has been initiated successfully.|
| transaction_completed    | The transaction has been completed successfully.        |
| user_info_retrieved      | User information has been retrieved successfully.       |
| keys_generated           | Keys have been generated successfully.                 |
| keys_rotated             | Keys have been rotated successfully.                   |
| webhook_registered       | The web hook has been registered successfully.          |
| successful_transaction       | Generic successful message.         |

##### Error Codes

| Code                       | Description                                                                                   | Resolution                                                      |
|----------------------------|-----------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| insufficient_balance       | Insufficient balance to complete the transaction.                                              | Ensure the user has sufficient funds before retrying.           |
| transaction_not_found      | The specified transaction ID does not exist.                                                   | Verify the transaction ID and try again.                        |
| invalid_uuid               | The provided UUID does not conform to the required format.                                     | Check the UUID format and correct it before retrying.           |
| permission_denied          | The authenticated user is not authorized to perform this operation.                            | Ensure the user has the necessary permissions before retrying.  |
| invalid_transaction_id     | The provided transaction ID is invalid.                                                        | Check the transaction ID and correct it before retrying.        |
| key_generation_failed      | Unable to generate keys due to server error.                                                   | Try again later. If the problem persists, contact support.      |
| key_rotation_failed        | Unable to rotate keys due to server error.                                                     | Try again later. If the problem persists, contact support.      |
| user_not_found             | The specified user does not exist.                                                             | Ensure the user ID is correct and exists in the system.         |
| authentication_error       | An authentication error occurred.                                                             | Ensure proper authentication credentials are provided.          |
| debit_failed               | Failed to debit from the balance.                                                             | Investigate the error and retry the transaction.                |
| credit_failed              | Failed to credit the balance.                                                                 | Investigate the error and retry the transaction.                |
| rate_limit_exceeded        | The user has exceeded the rate limit.                                                         | Wait for the rate limit to reset before making new requests.    |
| bad_request                | Malformed request. Potentially missing some fields.                                           | Check the request format and ensure all required fields are provided. |
| internal_error             | A generic error for all server-specific errors.                                                | Investigate the server error logs and contact support if needed. |
| aws_error                  | AWS-related error during the transaction.                                                      | Investigate AWS service logs and retry the transaction.         |
| encryption_error           | Error in data encryption or decryption process.                                                | Verify encryption settings and retry the transaction.           |
| user_not_found            | One or more users involved in the transaction do not exist.                                    | Ensure all involved user accounts exist and retry the transaction. |
| missing_fields             | Required fields are missing in the request.                                                    | Provide the missing fields and retry the transaction.           |

### Example API Response

**Success Response**

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

**Error Response**

```json
{
    "status": "failed",
    "code": "insufficient_balance",
    "message": "Insufficient balance to complete the transaction.",
    "details": "The user does not have enough balance in their account.",
    "timestamp": "2024-05-24T12:05:00Z",
    "data": {
        "uuid": "uuid_001",
        "signed_uuid": "signed_uuid_001",
        "status": "failed"
    }
}
```



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
    - Requests exceeding the limit receive a `429 Too Many Requests` response.
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
    - **URL:** `/api/v1/topup/initiate`
    - **Version:** v1

## **Administrative Tasks [INTERNAL]**

### **Tailscale VPN Integration**

For secure communication between Nil and service providers, Tailscale VPN is used. If hosting systems on AWS or other cloud providers, Tailscale can be integrated into their environments.

### Tailscale installation for local development and test environment

Here are installation notes for setting up Tailscale on a local development machine (Linux, Windows, or macOS) and signing in using Google identity for accessing servers secured behind a Tailscale network.

---

!!! warning
    https://nil.tailf4a43a.ts.net is only accessible via our specific tailscale tailnet, you cannot access it via internet and you cannot access it via any other tailscale organization.

# **Tailscale Installation and Setup Guide for Local Development**

## **1. Installation**

### **Linux**

1. **Install Tailscale:**
   - Open your terminal.
   - Run the following command based on your distribution:

   **Debian/Ubuntu:**
   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh
   ```

   **Fedora:**
   ```bash
   sudo dnf install tailscale
   ```

   **Arch Linux:**
   ```bash
   sudo pacman -S tailscale
   ```

   **Other distributions:**
   - Follow the installation instructions provided on [Tailscale’s Linux installation page](https://tailscale.com/kb/1033/install-linux).

2. **Start the Tailscale service:**
   ```bash
   sudo tailscale up
   ```

### **Windows**

1. **Download and Install Tailscale:**
   - Visit the [Tailscale download page](https://tailscale.com/download).
   - Download the Windows installer.
   - Run the installer and follow the on-screen instructions.

2. **Start Tailscale:**
   - After installation, Tailscale should automatically start. If not, search for "Tailscale" in the Start Menu and open it.

### **macOS**

1. **Download and Install Tailscale:**
   - Visit the [Tailscale download page](https://tailscale.com/download).
   - Download the macOS installer.
   - Open the downloaded `.dmg` file and drag Tailscale to your Applications folder.

2. **Start Tailscale:**
   - Open Tailscale from your Applications folder.

---

## **2. Sign in via Google Identity**

1. **Initiate the Sign-In Process:**
   - Once Tailscale is installed and running, a browser window will open for you to log in.
   - If not automatically opened, you can initiate sign-in by running:
     ```bash
     tailscale up
     ```
     (on Linux) or by clicking "Log in" in the Tailscale app on Windows/macOS.

2. **Choose Google as your Identity Provider:**
   - In the browser, select "Log in with Google."
   - Choose the Google account associated with your organization (you can request this one from Nilpay)

3. **Authorize Tailscale:**
   - Follow the prompts to authorize Tailscale to access your Google account.

4. **Complete the Login:**
   - After signing in, Tailscale will complete the setup on your local machine and connect to your Tailscale network.

---

## **3. Accessing Servers Secured by Tailscale**

1. **Connect to Your Tailscale Network:**
   - After logging in, your machine will be connected to the Tailscale network.
   - You can view connected devices and your network's IP addresses by running:
     ```bash
     tailscale status
     ```
     (on Linux) or by checking the Tailscale app on Windows/macOS.

2. **Access Servers:**
   - Use the Tailscale IP address (e.g., `100.x.x.x`) of the server you need to access (since we are using MagicDNS, you will use https://nil.tailf4a43a.ts.net for all interactions) 
   - You can connect via SSH, HTTP, or any other protocol your server supports, using the Tailscale IP address as if it were on your local network.

   **Example SSH Command:**
   ```bash
   ssh user@100.x.x.x
   ```

3. **Ensure Connectivity:**
   - Make sure your server allows connections from your Tailscale network and that your firewall rules are configured appropriately.

---




### **Tailscale with AWS VPC**

To integrate Tailscale with AWS VPC:

1. **Install Tailscale on EC2 Instances:**
    - SSH into your EC2 instance.
    - Install Tailscale by following the installation instructions for your OS from the [Tailscale documentation](https://tailscale.com/kb/1296/aws-reference-architecture).
2. **Authenticate Tailscale:**
    - Run `tailscale up` and authenticate using your Tailscale account.
3. **Configure VPC Peering:**
    - Use Tailscale's subnet routing feature to route traffic from the VPC to Tailscale nodes. This involves setting up Tailscale on a subnet router within your VPC.
4. **Setup Subnet Routes:**
    - Enable subnet routes on Tailscale by running `tailscale up --advertise-routes=10.0.0.0/16` (replace with your VPC CIDR).

### **Tailscale with Other Cloud Providers**

For other cloud providers like GCP, Azure, or DigitalOcean, the process is similar. Install Tailscale on your virtual machines, authenticate, and configure subnet routing as per the provider's network setup.


## **Data Field Rationale**

In our API responses, the `data` field will contain different information depending on the context of the API call. Here is a breakdown of the possible structures:

!!! note
    transaction_id is a system-generated ID (ksuid) for all transactions, while UUID is client-generated.
    Both can be used efficiently to retrieve a transaction's info.


### **Top-up Response Data**

- **Fields:**
    - `transaction_id`: Unique identifier for the transaction, generated by the Nil system.
    - `status`: Status of the transaction (e.g., pending, completed).
    - `amount`: Amount involved in the transaction.
    - `currency`: Currency of the transaction.
    - `uuid`: Unique identifier for the request.
    - `signed_uuid`: Signed unique identifier for the request.
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

### **General API Response Data**

- **Description:** This field will be used to return specific data relevant to the context of the API call, ensuring that responses are informative and useful for the API consumers.
- **Note:** Always refer to the specific API endpoint documentation to understand the exact structure and contents of the `data` field for that endpoint.


**OpenAPI Schema**

```yaml
openapi: 3.0.0
info:
  title: Non-blocking Transaction API
  version: 1.0.0
paths:
  /api/v1/topup/initiate:
    post:
      summary: Initiate Top-up
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                user_id:
                  type: string
                amount:
                  type: number
                service_provider:
                  type: string
                uuid:
                  type: string
                signed_uuid:
                  type: string
                timestamp:
                  type: string
                currency:
                  type: string
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                  code:
                    type: string
                  message:
                    type: string
                  data:
                    type: object
                    properties:
                      transaction_id:
                        type: string
                      status:
                        type: string
                      amount:
                        type: number
                      currency:
                        type: string
                      uuid:
                        type: string
                      signed_uuid:
                        type: string
        '400':
          description: Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /api/v1/cashout/initiate:
    post:
      summary: Initiate Cash Out
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                uuid:
                  type: string
                signed_uuid:
                  type: string
                timestamp:
                  type: string
                from_account:
                  type: string
                to_account:
                  type: string
                amount:
                  type: number
                currency:
                  type: string
                notes:
                  type: string
                service_provider:
                  type: string
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                  code:
                    type: string
                  message:
                    type: string
                  data:
                    type: object
                    properties:
                      transaction_id:
                        type: string
                      status:
                        type: string
                      amount:
                        type: number
                      currency:
                        type: string
                      timestamp:
                        type: string
                      uuid:
                        type: string
                      signed_uuid:
                        type: string
        '400':
          description: Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /api/v1/transaction/status:
    post:
      summary: Check Transaction Status
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                uuid:
                  type: string
                signed_uuid:
                  type: string
                timestamp:
                  type: string
                service_provider:
                  type: string
                original_uuid:
                  type: string
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                  code:
                    type: string
                  message:
                    type: string
                  data:
                    type: object
                    properties:
                      original_uuid:
                        type: string
                      status:
                        type: string
                      type:
                        type: string
                      details:
                        type: string
                      last_checked:
                        type: string
                      uuid:
                        type: string
                      signed_uuid:
                        type: string
        '400':
          description: Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /api/v1/user:
    post:
      summary: User Info
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                user_id:
                  type: string
                service_provider:
                  type: string
                uuid:
                  type: string
                signed_uuid:
                  type: string
                timestamp:
                  type: string
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                  code:
                    type: string
                  message:
                    type: string
                  data:
                    type: object
                    properties:
                      user_id:
                        type: string
                      fullname:
                        type: string
                      status:
                        type: string
        '400':
          description: Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /api/v1/keys/generate:
    post:
      summary: Generate Keys
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                  code:
                    type: string
                  message:
                    type: string
                  data:
                    type: object
                    properties:
                      public_key:
                        type: string
                      private_key:
                        type: string
                      uuid:
                        type: string
                      signed_uuid:
                        type: string
        '400':
          description: Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
  /api/v1/keys/rotate:
    post:
      summary: Rotate Keys
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                  code:
                    type: string
                  message:
                    type: string
                  data:
                    type: object
                    properties:
                      new_public_key:
                        type: string
                      new_private_key:
                        type: string
                      uuid:
                        type: string
                      signed_uuid:
                        type: string
        '400':
          description: Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
components:
  schemas:
    Error:
      type: object
      properties:
        status:
          type: string
        code:
          type: string
        message:
          type: string
        details:
          type: string
        timestamp:
          type: string
        uuid:
          type: string
        signed_uuid:
          type: string
```