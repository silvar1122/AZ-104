## Scenario ##

A company stores sensitive business documents in Azure Blob Storage. While Azure Storage automatically encrypts data at rest using Microsoft-managed encryption, the company's security team requires greater control over the encryption keys used to protect highly sensitive data.

The security team wants to ensure that the encryption key can be controlled, rotated, and revoked by the organization rather than being entirely managed by Microsoft.

To achieve this, I implemented a customer-managed key (CMK) solution using Azure Key Vault and Azure Storage Encryption Scopes.

## Business requirement ##

The company has two categories of data:

Azure Storage Account
│
├── General Data
│     └── Standard Azure Storage encryption
│
└── Sensitive Data
      └── Customer-managed encryption scope
            └── Azure Key Vault RSA key

Sensitive data must be protected using a customer-managed RSA key stored in Azure Key Vault.

The solution must also follow the principle of least privilege, meaning the Storage Account should only receive the permissions it requires to use the encryption key.
