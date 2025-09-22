---
title: "azapi_resource"
type: docs
weight: 200
---

# The general-purpose everything* resource

> *(\*) almost everything*

The `azapi_resource` resource is the core building block of the AzAPI provider. It allows you to create, update, and delete any Azure management-plane resource by specifying its type, name, location, and properties. The `body` argument is used to define the resource's properties as a dynamic HCL object.

## Almost everything?

The reason for the *almost everything* asterisk above is that this resource does not cover:

1. Data plane resources, see [`azapi_data_plane_resource`](azapi_data_plane_resource.md).
2. HTTP POST requests for triggering an action on a resource. E.g. start VM, list storage account keys, etc. See [`azapi_resource_action`](azapi_resource_action.md) for that.
