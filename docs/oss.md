# OSS specific documentation

# Cash Out API Specification

## Endpoint
`POST /api/v1/cashout/initiate`

## Request Headers
- `Content-Type: application/json`

## Request Body
```json
{
  "uuid": "string",
  "signed_uuid": "string",
  "to_account": "string",
  "amount": number,
  "currency": "string",
  "service_provider": "string",
  "cashout_provider": "string"
}
```

### Field Descriptions
- `uuid`: A unique identifier for the request (UUIDv4 format)
- `signed_uuid`: Base64 encoded signature of the UUID
- `to_account`: The recipient's account number
- `amount`: The amount to be transferred (positive number)
- `currency`: The currency of the transaction (e.g., "SDG")
- `service_provider`: The identifier for the service provider
- `cashout_provider`: The cash out provider (either "nil" or "bok")

## Response

### Success Response
- Status Code: 200 OK
- Content-Type: application/json

```json
{
  "status": "success",
  "code": "cashout_initiated",
  "message": "Cash out request processed successfully.",
  "data": {
    "transaction_id": "string",
    "status": "pending",
    "amount": number,
    "currency": "string",
    "timestamp": "string",
    "uuid": "string",
    "signed_uuid": "string"
  }
}
```

### Error Responses
- Status Code: 400 Bad Request, 401 Unauthorized, or 500 Internal Server Error
- Content-Type: application/json

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

## Key Generation and Signing Process

1. Key Generation:
   - Generate an RSA key pair (2048 bits recommended)
   - Store the private key securely
   - Share the public key with the API provider

2. Request Signing:
   - Generate a UUIDv4 for each request
   - Hash the UUID using SHA-256
   - Sign the hashed UUID using the private key with PKCS1v15 padding
   - Base64 encode the resulting signature

3. Signature Verification (Server-side):
   - Decode the Base64 `signed_uuid`
   - Hash the provided `uuid` using SHA-256
   - Verify the signature using the stored public key

## Error Codes
- `malformed_request`: Invalid request format
- `missing_fields`: Required fields are missing or invalid
- `invalid_uuid`: Provided UUID is not a valid UUIDv4
- `duplicate_request`: A request with the same UUID has already been processed
- `service_provider_not_found`: The specified service provider does not exist
- `user_not_found`: One or more users involved in the transaction do not exist
- `authentication_error`: Signature verification failed
- `cashout_provider_not_found`: Invalid cash out provider specified
- `currency_not_supported`: The specified currency is not supported
- `transaction_failed`: Unable to process the cash out request

## Notes
- All requests must use HTTPS
- The API uses idempotency based on the UUID to prevent duplicate transactions
- The service provider must be pre-registered and have a valid public key stored on the server
- The cash out provider must be either "nil" or "bok"
- Currently, only "SDG" is supported as a valid currency


# GetUserInfo API

## Endpoint
`POST /api/v1/user/info`

## Request Headers
- `Content-Type: application/json`

## Request Body
```json
{
  "uuid": "string",
  "signed_uuid": "string",
  "mobile": "string",
  "service_provider": "string"
}
```

### Field Descriptions
- `uuid`: A unique identifier for the request (UUIDv4 format)
- `signed_uuid`: Base64 encoded signature of the UUID
- `mobile`: The mobile number of the user whose information is being requested
- `service_provider`: The identifier for the service provider

## Response

### Success Response
- Status Code: 200 OK
- Content-Type: application/json

```json
{
  "status": "success",
  "code": "user_info_retrieved",
  "message": "User info retrieved successfully.",
  "data": {
    "transaction_id": "string",
    "timestamp": "string",
    "mobile": "string",
    "fullname": "string",
    "tenant_id": "string",
    "is_verified": boolean,
    "account_id": "string",
    "address": "string"
  }
}
```

### Error Responses
- Status Code: 400 Bad Request, 401 Unauthorized, or 500 Internal Server Error
- Content-Type: application/json

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

## Error Codes
- `malformed_request`: Invalid request format
- `missing_fields`: Required fields are missing or invalid
- `service_provider_not_found`: The specified service provider does not exist
- `authentication_error`: Signature verification failed
- `transaction_failed`: Unable to retrieve user information

## Notes
- All requests must use HTTPS
- The service provider must be pre-registered and have a valid public key stored on the server
- The `is_verified` field in the response is currently always set to `true`
- The `address` field in the response is currently always set to "N/A"


# Cash Out Webhook API Documentation

## Overview
This webhook sends real-time updates about the status of cash out transactions. Subscribers will receive a POST request to their specified URL whenever a transaction status changes.

## Endpoint
`POST {subscriber_specified_url}`

## Request Headers
- `Content-Type: application/json`


## Payload
```json
{
  "transaction_id": "string",
  "amount": number,
  "status": "string",
  "timestamp": "string",
  "from_account": "string",
  "to_account": "string",
  "uuid": "string",
  "cashout_provider": "string",
  "comment": "string"
}
```

### Field Descriptions
- `transaction_id`: A unique identifier for the transaction (system generated, KSUID)
- `amount`: The transaction amount.
- `status`: The current status of the transaction (e.g., "Completed", "Pending", "Failed").
- `timestamp`: The ISO 8601 formatted date and time when the status was updated.
- `from_account`: The source account identifier (may be a generic identifier for escrow accounts).
- `to_account`: The destination account identifier (partially masked for privacy).
- `cashout_provider`: The identifier for the cash out provider (if applicable).
- `comment`: A brief description or comment about the transaction.
- `uuid`: client generated uuid for the request

## Sample Payload
```json
{
  "transaction_id": "2l9L6Ef49M1jWGa8ZNQ2dUZigVQ",
  "amount": 1,
  "status": "Completed",
  "timestamp": "2024-08-25T11:59:29Z",
  "from_account": "NIL_ESCROW_ACCOUNT",
  "to_account": "096****6869",
  "cashout_provider": "nil",
  "comment": "Cashout",
  "uuid": "your-generated-valid-uuid-v4",
}
```

## Possible Status Values
- `Pending`: The transaction has been initiated but not yet completed.
- `Completed`: The transaction has been successfully processed.
- `Failed`: The transaction could not be completed.
- `Cancelled`: The transaction was cancelled before completion.

## Security Considerations

### HTTPS
All webhook communications should be done over HTTPS to ensure data privacy in transit.


## Best Practices for Webhook Consumers

1. Respond quickly to the webhook with a 2xx status code. Process the data asynchronously if needed.
2. Implement retry logic with exponential backoff for failed webhook deliveries.
3. Store the `transaction_id` to prevent duplicate processing in case of repeated webhook deliveries.
4. Validate all incoming data before processing.
5. Be prepared to handle occasional out-of-order webhook deliveries.

## Rate Limiting
Webhooks are sent as events occur. There is no specific rate limit, but consumers should be prepared to handle varying volumes of incoming webhooks.

## Support
For any issues or questions regarding the webhook integration, please contact our support team at info@pynil.com.