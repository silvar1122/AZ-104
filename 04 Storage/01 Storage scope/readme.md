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
