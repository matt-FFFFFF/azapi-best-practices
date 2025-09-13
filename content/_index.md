---
title: "The AzAPI Terraform Provider: Best Practices for Success"
type: docs
---

# The AzAPI Terraform Provider: Best Practices for Success

Welcome to the comprehensive guide for using the AzAPI Terraform provider effectively. This book covers best practices, common patterns, and advanced techniques for managing Azure resources with the AzAPI provider.

## Why AzAPI?

As you travel along your Terraform on Azure journey, it is ***absolutely inevitable*** that you will eventually encounter a situation where HashiCorp's AzureRM provider does not support a resource you need to manage. When you reach this point you will have two options:

1. Roll up your sleeves, learn Go, modify the provider, create a pull-request, and hope that it will be merged; or
2. Use your new best friend, the AzAPI provider, where all the resources are supported!

Additionally, with version 2.0 of AzAPI we have some unique and awesome features, such as configurable retry logic, the ability to invoke one-shot actions on resources, and write-only attribute support for any Azure resource.

I make no bones about it, I have been a proponent of the AzAPI provider since its inception. It started with a desire to be freed from some of the artificial constraints of AzureRM, and has grown into a powerful tool with amazing features that I use for all my Azure infrastructure as code (IaC) projects. Maybe after reading this book you will feel the same way!

As with everything, with great power comes great responsibility. The AzAPI provider's design flexibility does have a steeper learning curve than its fully curated cousin, AzureRM. You will get to know the ins and outs of the Azure REST APIs as you work with the AzAPI provider. *This is a good thing!* It will make you a more capable and confident Terraform on Azure user. You will be able to do things in HCL[^1] that previously had to be authored in Go. This book aims to equip you with the knowledge and best practices to harness the full potential of the AzAPI provider while avoiding common pitfalls.

## What You'll Learn

- The AzAPI provider and its resources
- How to write reusable modules with AzAPI
- Advanced techniques for managing Azure resources with AzAPI
  - Locks (mutexes)
  - Mimicking AzureRM behaviour in AzAPI
  - Retry
  - How to construct the `body` argument when things get complex
  - How to use `azapi_resource_action` to invoke one-shot actions on resources

## Prerequisites

Readers should have an intermediate understanding of Terraform and its features. At a minimum, you should be familiar with the following concepts:

- Resources
- Data sources
- Variables
- Outputs
- Locals
- Types
- Using for loops to create lists and maps
- for_each & count
- The ternary operator: `<boolean expression> ? <value if true> : <value if false>`

[About the Author →](docs/author.md)

[^1]: [HashiCorp Configuration Language (HCL)](https://www.terraform.io/language)
