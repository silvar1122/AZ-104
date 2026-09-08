## Scenario ##
Organizations often store different types of data within the same Azure Storage account. While Azure Storage provides encryption at rest by default, some datasets may require a more granular encryption boundary because of security, compliance, or key-management requirements.

In this lab, a security engineer is responsible for protecting sensitive data stored in Azure Blob Storage. The organization already uses an Azure Storage account, but wants to isolate the encryption configuration for a specific dataset without creating a separate storage account.

The engineer will:

* **Create an Azure Storage account.**
* **Create an encryption scope within the storage account.**
* **Configure the encryption scope to use a defined encryption boundary.**
* **Create a Blob Storage container associated with the encryption scope.**
* **Upload a test blob into the protected container.**
* **Verify that the container and blob are using the intended encryption scope.**
* **Document the configuration and security design for audit and portfolio purposes**


## Security Objective ##

The primary objective is to demonstrate data-at-rest protection with a dedicated encryption scope while maintaining a single Azure Storage account.

This provides a practical example of how a cloud security engineer can implement defense-in-depth, encryption isolation, and controlled data protection within Azure Blob Storage.

## Security Challenge & Resolution ##
## Challenge: Storage Account Network Access Restriction ##

During the lab, I encountered an access issue when attempting to open and access the Blob Storage container through the Azure Portal.

The Storage Account was configured with restricted network access rather than allowing access from all public networks. As a result, my client connection was not permitted to access the storage data, even though I had successfully created the storage account, container, and encryption scope.

This demonstrated an important distinction between authentication/authorization and network access control:

Having permission to access the Storage Account does not automatically mean the network connection is allowed.
Azure Storage evaluates network restrictions before allowing access to the storage data.
The Storage Account firewall can restrict access to selected virtual networks or public IP addresses.
Resolution

I identified the public IP address of my current network and added it to the Storage Account's Networking → Firewalls and virtual networks → Public network access configuration.

The configuration was changed to allow access from selected networks, with my current public IP address added as an allowed network.

## Security Lesson Learned ##

This troubleshooting experience demonstrated that Azure Storage security is based on multiple layers of controls.

                Azure Storage Security
                        │
        ┌───────────────┼────────────────┐
        ▼               ▼                ▼
   Network Access   Authentication   Authorization
        │               │                │
 Public IP / VNet    Identity        RBAC / SAS
        │
        ▼
   Storage Firewall
        │
        ▼
   Encryption
        │
        ▼
 Encryption Scope
