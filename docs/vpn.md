### **Tailscale VPN Integration**

For secure communication between Nil and service providers, Tailscale VPN is used. If hosting systems on AWS or other cloud providers, Tailscale can be integrated into their environments.

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

In our API responses, the `data` field will contain different information depending on the context of the API call. Here is a breakdown of the possible structures:

### **Top-up Response Data**

- **Fields:**
    - `transaction_id`: Unique identifier for the transaction.
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