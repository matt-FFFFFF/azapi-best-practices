---
title: "azapi_resource"
type: docs
weight: 100
---

# The general-purpose everything* resource

> *(\*) almost everything*

The `azapi_resource` resource is the core building block of the AzAPI provider. It allows you to create, update, and delete any Azure management-plane resource by specifying its type, name, location, and properties. The `body` argument is used to define the resource's properties as a dynamic HCL object.
