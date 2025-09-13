---
title: "Chapter 1: The AzAPI Provider and its resources"
type: docs
weight: 100
---

# Chapter 1: The AzAPI Provider and its resources

The AzAPI provider is a powerful tool that warrants a deep understanding to be used effectively. This chapter introduces the AzAPI provider, its design philosophy, and its core resources. It is built on top of the Azure REST APIs, providing a consistent and unified interface for managing Azure resources.

## Design Philosophy and comparison to AzureRM

The AzAPI provider is designed to be a low-level, flexible, and extensible solution for managing Azure resources. It is a general-purpose provider that can manage any Azure management-plane resource. This is a crucial difference to AzureRM, which provides resource types for specific Azure services. AzAPI's general-purpose approach allows it to support new and preview Azure services as soon as they are available in ARM, without needing to wait for the provider to be updated.

AzAPI has limited support for data-plane resources, which are resources that manage the data within a service, such as the contents of a storage account. In general, AzureRM provides richer support for data-plane resources.

| Feature | AzAPI | AzureRM |
| :--- | :--- | :--- |
| Management-plane resources | ✅ | ✅ (not 100%) |
| Data-plane resources | ⚠️ Limited | ✅ |
| Configurable retry logic | ✅ | ❌ |
| Customization of HTTP requests | ✅ | ❌ |
| Full ephemeral and write-only attribute support | ✅ | ❌ |


{{% hint info %}}
**Management plane vs. data plane**

The Azure management plan is the set of APIs used to manage Azure resources, such as creating, updating, and deleting resources. The data plane is the set of APIs used to interact with the data stored in those resources.

In terms of endpoints, management plane endpoints in Azure Public have the format `https://management.azure.com/`, while data plane endpoints vary depending on the specific service and resource.
{{% /hint %}}

## IDE Setup

To make the most of your AzAPI experience, it's highly-recommended to use an IDE with the following two extensions. I have linked to Visual Studio Code, but other IDEs may have similar extensions:

1. [Microsoft Terraform](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureterraform)
2. [HashiCorp Terraform](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)

Microsoft Terraform is the replacement extension for Azure Terraform now that the [msgraph](https://github.com/microsoft/terraform-provider-msgraph) provider has been released.

The language server provides completion help for the resource types, api versions, and the resource body. Using it will make your life much easier when working with AzAPI!

{{% hint info %}}
**Language server**

If you do not use an IDE with VSIX extension support, you can download the language server directly from GitHub and configure your IDE to use it. The Microsoft Terraform language server is [published here](https://github.com/Azure/ms-terraform-lsp).
{{% /hint %}}

## Provider Resources

We will cover these in more detail in subsequent pages, but here is a brief overview of the resources in the AzAPI provider:

### Managed resources

1. `azapi_resource`
1. `azapi_update_resource`
1. `azapi_resource_action`
1. `azapi_data_plane_resource`

### Data sources

1. `data.azapi_resource`
2. `data.azapi_resource_action`
3. `data.azapi_resource_id`
4. `data.azapi_resource_list`

### Ephemeral resources

1. `ephemeral.azapi_resource_action`

### Functions

1. `build_resource_id`
1. `extension_resource_id`
1. `management_group_resource_id`
1. `parse_resource_id`
1. `resource_group_resource_id`
1. `subscription_resource_id`
1. `tenant_resource_id`
1. `unique_string`

Let's look at these resources in detail, starting with the most important one, [`azapi_resource`](azapi_resource.md).
