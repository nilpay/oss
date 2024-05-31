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