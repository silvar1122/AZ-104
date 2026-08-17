# Azure Point-to-Site VPN with VPN Gateway

## Objective

The objective of this lab was to configure a **Point-to-Site (P2S) VPN connection using Azure VPN Gateway** and securely connect a remote Windows client to resources inside an Azure Virtual Network.

The lab was designed to help me understand how remote users can securely access private Azure resources without exposing those resources directly to the public Internet.

---

## Architecture

The lab consists of a remote Windows client connecting to an Azure Virtual Network through an Azure VPN Gateway.

The VPN connection uses **OpenVPN** and **Microsoft Entra ID authentication**.

### Network design

* **Virtual Network:** `vnet-p2s-vpn`
* **VNet address space:** `10.0.0.0/16`
* **VM subnet:** `10.0.1.0/24`
* **Gateway subnet:** `10.0.255.0/27`
* **P2S client address pool:** `172.16.201.0/24`

### Traffic flow

```text
Windows Client
      |
      | Encrypted OpenVPN connection
      |
   Internet
      |
      v
Azure VPN Gateway
      |
      v
GatewaySubnet
      |
      v
Azure VNet
      |
      v
VMSubnet
      |
      v
Azure VM
```

The VPN client receives an IP address from the P2S client address pool and can then communicate with resources in the Azure VNet according to the configured routing and security rules.

---

## Resources

The following Azure resources were deployed for this lab:

| Resource           | Configuration           |
| ------------------ | ----------------------- |
| Resource Group     | `rg-az104-p2s-vpn`      |
| Virtual Network    | `vnet-p2s-vpn`          |
| VNet Address Space | `10.0.0.0/16`           |
| VM Subnet          | `10.0.1.0/24`           |
| GatewaySubnet      | `10.0.255.0/27`         |
| Virtual Machine    | `vm-p2s-test`           |
| VPN Gateway        | `vng-p2s-vpn`           |
| VPN Type           | Route-based             |
| P2S Protocol       | OpenVPN                 |
| Authentication     | Microsoft Entra ID      |
| P2S Address Pool   | `172.16.201.0/24`       |
| Public IP          | Assigned to VPN Gateway |

---

## Configuration

### Virtual Network

I created an Azure Virtual Network using the address space:

```text
10.0.0.0/16
```

The VNet was divided into separate subnets for the virtual machine and VPN Gateway.

The subnet structure was:

```text
vnet-p2s-vpn
│
├── VMSubnet
│   └── 10.0.1.0/24
│
└── GatewaySubnet
    └── 10.0.255.0/27
```

The separation of the subnets allows the VPN Gateway and workload resources to have their own dedicated network ranges.

---

### GatewaySubnet

I created a dedicated subnet named:

```text
GatewaySubnet
```

with the address range:

```text
10.0.255.0/27
```

The `GatewaySubnet` is required by Azure VPN Gateway and provides the network space used by the gateway.

I did not deploy virtual machines or other workloads into this subnet.

---

### Virtual Machine

I deployed a test virtual machine inside:

```text
VMSubnet
10.0.1.0/24
```

The VM was used as the private Azure resource that I would access through the P2S VPN connection.

The VM was accessed using its **private IP address**, rather than relying on a public IP for the VPN connectivity test.

---

### VPN Gateway

I created an Azure Virtual Network Gateway with the following configuration:

```text
Name: vng-p2s-vpn
Gateway type: VPN
VPN type: Route-based
SKU: VpnGw1
```

The VPN Gateway was associated with the `vnet-p2s-vpn` virtual network and a public IP address.

I kept **Active-Active mode disabled** for this lab because the objective was to understand the basic P2S VPN configuration rather than implement a highly available gateway architecture.

---

### P2S Configuration

I configured Point-to-Site VPN on the VPN Gateway.

The client address pool was configured as:

```text
172.16.201.0/24
```

This address range is used to assign private IP addresses to clients connecting through the P2S VPN.

For example, a connected client could receive:

```text
172.16.201.5
```

The Azure VM remained on the VNet address space, for example:

```text
10.0.1.4
```

This demonstrates that the VPN client and Azure resources can use different private address ranges while communicating through the VPN gateway.

---

### Authentication

I configured **Microsoft Entra ID** as the authentication method for the Point-to-Site VPN.

The VPN tunnel type was configured as:

```text
OpenVPN
```

The Azure VPN Client was used on the Windows client to establish the VPN connection.

The authentication flow was:

```text
Windows Client
      |
      v
Azure VPN Client
      |
      v
Microsoft Entra ID
      |
      v
Azure VPN Gateway
      |
      v
Azure VNet
```

This allows the VPN connection to use organizational identity authentication instead of relying only on a shared VPN credential.

---

## Testing

After completing the configuration, I downloaded the VPN client configuration from the Azure VPN Gateway and imported it into the **Azure VPN Client** on my Windows computer.

I then authenticated using my Microsoft Entra ID account and established the VPN connection.

### VPN connection

The Azure VPN Client successfully established a connection to the Azure VPN Gateway.

The client received an IP address from the configured P2S address pool:

```text
172.16.201.0/24
```

### Private resource access

After establishing the VPN connection, I tested access to the Azure VM using its private IP address.

The expected traffic path was:

```text
Client
172.16.201.x
      |
      | P2S VPN
      v
VPN Gateway
      |
      v
Azure VNet
      |
      v
VM
10.0.1.x
```

This demonstrated the purpose of the P2S VPN: allowing the remote client to reach a private Azure resource through the VPN Gateway.

### Evidence

Screenshots showing the configuration and successful VPN connection are stored in the `screenshots` directory.

---

## Troubleshooting

During the configuration, I learned that successful VPN connectivity depends on several components working together.

I checked the following when troubleshooting connectivity:

* VPN Gateway provisioning status
* P2S configuration
* VPN client configuration
* Microsoft Entra ID authentication
* Client VPN IP address
* Azure VM private IP address
* Network Security Group rules
* Windows Firewall rules
* Network routing

One important lesson was that a failed `ping` test does not necessarily mean that the VPN connection is broken. ICMP traffic can be blocked by the VM's operating system firewall even when the VPN tunnel itself is working correctly.

---

## Security Considerations

The purpose of the P2S VPN configuration was to provide secure access to private Azure resources without exposing the VM directly to the public Internet.

Security considerations included:

* Using an encrypted VPN tunnel.
* Using Microsoft Entra ID for authentication.
* Accessing the VM through its private IP address.
* Keeping the VPN Gateway in the dedicated `GatewaySubnet`.
* Using Network Security Groups to control network traffic where required.
* Avoiding unnecessary public exposure of the Azure VM.
* Not committing VPN configuration files, credentials, certificates, or other sensitive information to GitHub.

---

## What I Learned

Through this lab, I learned how Point-to-Site VPN works in Azure and how Azure VPN Gateway provides secure connectivity between an individual remote client and an Azure Virtual Network.

Key concepts I learned include:

* The difference between **Point-to-Site and Site-to-Site VPN**.
* The purpose of the **GatewaySubnet**.
* How an Azure **VPN Gateway** terminates VPN connections.
* How a **P2S address pool** assigns IP addresses to VPN clients.
* How **Microsoft Entra ID** can be used to authenticate VPN users.
* How **OpenVPN** is used for the P2S tunnel in this configuration.
* How to use the **Azure VPN Client** to establish a connection.
* How private IP addresses can be used to access Azure resources through a VPN.
* How NSGs and host firewalls can affect connectivity.
* The importance of troubleshooting each layer of the network rather than assuming the VPN Gateway is the only possible point of failure.

This lab gave me practical experience with Azure networking and helped me understand how secure remote connectivity can be implemented using Azure VPN Gateway.
