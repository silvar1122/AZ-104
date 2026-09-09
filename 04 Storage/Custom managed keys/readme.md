## Scenario ##

A company stores sensitive business documents in Azure Blob Storage. While Azure Storage automatically encrypts data at rest using Microsoft-managed encryption, the company's security team requires greater control over the encryption keys used to protect highly sensitive data.

The security team wants to ensure that the encryption key can be controlled, rotated, and revoked by the organization rather than being entirely managed by Microsoft.

To achieve this, I implemented a customer-managed key (CMK) solution using Azure Key Vault and Azure Storage Encryption Scopes.

## Business requirement ##

The company has two categories of data:

Azure Storage Account
* **General Data** *
      * Standard Azure Storage encryption
* **Sensitive Data**
      * Customer-managed encryption scope
            * Azure Key Vault RSA key

Sensitive data must be protected using a customer-managed RSA key stored in Azure Key Vault.

The solution must also follow the principle of least privilege, meaning the Storage Account should only receive the permissions it requires to use the encryption key.


## Objectives ##

In this lab, I will:

* **Create an Azure Key Vault.**
* **Enable purge protection on the Key Vault.**
* **Create a customer-managed RSA key.**
* **Enable a system-assigned managed identity on the Storage Account.**
* **Grant the Storage Account identity the minimum required Key Vault permissions.**
* **Create a customer-managed encryption scope in Azure Storage.**
* **Associate the Key Vault RSA key with the encryption scope.**
* **Create a dedicated Blob container for sensitive data.**
* **Apply the CMK-backed encryption scope to the container.**
* **Upload a test blob.**
* **Verify that the configuration is working correctly.**
