---
title: "Configuring the provider"
type: docs
weight: 100
---

# Configuring the provider

Before we can make the provider do work, we need to configure it. I am not going to repeat the documentation that is already available on the [provider page](https://registry.terraform.io/providers/Azure/azapi/latest/docs). Instead, I will highlight some of the more interesting and useful features of the provider configuration.

## Authentication

For local work, then Azure CLI authentication is the easiest to set up. Just run `az login` and you are good to go. The provider will automatically pick up the credentials from your Azure CLI session. Boom.

For CI/CD pipelines, you should not be using anything other than Workload Identity Federation, also known as OpenID Connect (OIDC). This is the most secure way to authenticate, as it avoids the need to manage service principal credentials. Instead, a short-lived token is issued by a trusted identity provider (IdP) and exchanged for Azure credentials.

It is good practice to configure two identities for your pipelines: one for plan and one for apply. The plan identity should have read-only access to the resources it needs to read. The apply identity should have the necessary permissions to create, update, and delete resources, as well as register resource providers.

You can read more about this in the [official documentation](https://learn.microsoft.com/entra/workload-id/workload-identity-federation).

### How to set up Workload Identity Federation

First, you need to create an identity in Entra. This can be either a user-assigned managed identity or application registration. I strongly recommend using a user-assigned managed identity. Why? Because it is subject to the same management API as other Azure resources, meaning you can manage it easily without having to have any Entra administrative roles.

Speaking of Entra roles, they are somewhat inflexible when it comes to application registrations. If you do want to use an application registration, then you will need to have either the Application Administrator or Application Developer role in Entra[^1]. This presents a problem in either case. The Application Developer role seems adequate upon first glance, until you come to learn that you cannot delete the registrations that you have created. If you want to fully manage the lifecycle of the application registration, you will need to have the Application Administrator role. This is a highly-privileged role, allowing creation and management of all aspects of app registrations and enterprise apps. You should be extremely judicious about granting this role.

By now hopefully you have seen the light and are convinced to use a user-assigned managed identity. However there is one risk that I want to point out. If an administrator assigned the identity to a Azure resource, for example a virtual machine, then anyone with access to that resource can use the identity to authenticate to Azure. Once denyAssignments are generally available, this risk can be mitigated by creating a deny assignment on the resource that forbids the identity assign action (`Microsoft.ManagedIdentity/userAssignedIdentities/assign/action`).

### Adding federated credentials to the identity

In the Azure Portal, there is a simplified interface for adding federated credentials to a managed identity. At the time of writing this supports GitHub actions and Kubernetes service accounts. However all you really need to supply is the following information:

- The issuer URL of the IdP
- The subject claim that will be present in the token
- The audiences that the token is intended for

The issuer URL is the thing that allows Entra to trust the tokens issued by the IdP. For example, in GitHub actions the issuer URL is `https://token.actions.githubusercontent.com`. For other IdPs, you will need to look up the issuer URL in their documentation. Trust is established by Entra fetching the OpenID Connect metadata document from the issuer URL. This is done using well-known URLs, for example `https://token.actions.githubusercontent.com/.well-known/openid-configuration` lists the metadata for GitHub actions. Therein is link to the public key set that Entra will use to verify the token signature. For GitHub actions, the key set URL is `https://token.actions.githubusercontent.com/.well-known/jwks`. This is how Entra can trust the signature of the token issued by the IdP.

The subject claim is a string that uniquely identifies the workload. Again, this depends on the IdP you are using. In GitHub actions, by default this can refer to a branch, tag, or environment. For example, `repo:<owner>/<repo>:environment:test` will match tokens issued to the test environment of the specified repository.

It's often possible to customize the subject claims to suit your needs. For example in GitHub Actions, if you wanted to ensure that your identity could be used only in a specific, managed workflow, you can add the `job_workflow_ref` claim to the subject. This will ensure that only tokens issued to that specific workflow can be used to authenticate.

Finally, the audience is a list of strings that identify the intended recipients of the token. For Azure, this should always be `["api://AzureADTokenExchange"]`.

If you are using Azure DevOps, you will configure the federated credential using a service connection. The process is documented

Using Workload Identity Federation is a game-changer for secure authentication in CI/CD pipelines. It works with most CI/CD systems:

- [Azure DevOps](https://learn.microsoft.com/azure/devops/pipelines/release/configure-workload-identity?view=azure-devops&tabs=managed-identity)
- GitHub Actions (see [here](https://docs.github.com/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-azure)
- [GitLab](https://docs.gitlab.com/ci/cloud_services/azure/)
- [Terraform Cloud](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials/azure-configuration)

{{% hint info %}}
In all cases you may use either an application registration or a user-assigned managed identity.
{{% /hint %}}

### Creating a role assignment

Once you have created the identity and added a federated credential, you need to assign it a role in Azure. The principle of least privilege should be applied here. Start with the most restrictive role that allows the identity to perform its tasks. You can always add more roles later if needed.

[^1]: Assuming you have the '*Users can register applications*' [setting disabled](https://learn.microsoft.com/entra/identity/role-based-access-control/delegate-app-roles#to-disable-the-default-ability-to-create-application-registrations-or-consent-to-applications) in Entra.

### Provider configuration

Here is the minimum required provider configuration for OpenID Connect:

```hcl
provider "azapi" {
  subscription_id = "00000000-0000-0000-0000-000000000000"
  tenant_id       = "00000000-0000-0000-0000-000000000000"
  client_id       = "00000000-0000-0000-0000-000000000000"
  use_oidc        = true
}
```

However this is somewhat inflexible to hard-code the values. A better approach is to use environment variables:

| Variable               | Example Value                          |
| :--------------------- | :------------------------------------  |
| `ARM_SUBSCRIPTION_ID`  | `00000000-0000-0000-0000-000000000000` |
| `ARM_TENANT_ID`        | `00000000-0000-0000-0000-000000000000` |
| `ARM_CLIENT_ID`        | `00000000-0000-0000-0000-000000000000` |
| `ARM_USE_OIDC`         | `true`                                 |

{{% hint info %}}
In addition, you should ensure that the `oidc_request_token` and `oidc_request_url` are set. This allows the provider to refresh the token if it expires during the run! If you are using GitHub Actions or Azure DevOps, this is done automatically via the environment.

If using another CI/CD platform, you may set these via the environment variables:

- `ARM_OIDC_REQUEST_TOKEN`
- `ARM_OIDC_REQUEST_URL`
{{% /hint %}}

## Preflight - Catch errors at plan time

The `enable_preflight` attribute (`ARM_ENABLE_PREFLIGHT` environment variable) is a useful feature that can help catch errors early. When enabled, the provider will make a call to the Azure Resource Manager (ARM) API to try and validate the payload before making any changes. The API called is the `Microsoft.Resources/validateResources` action.

This setting can help identify issues with Azure Policy (finally!). Meaning the plan will fail if the resource creation or update would be blocked by a policy.

## Ignoring no-op changes

Since version `v2.5.0`, the `ignore_no_op_changes` attribute has been added to the provider. The default is `true`, which means that the provider will ignore changes that do not result in any actual changes to the resource. Some good examples of this:

1. API versions changes
1. Adding or removing properties that match state

The way this works is that, during the plan, the provider will fetch the current state of the resource from Azure and compare it to the desired state. If there are no differences, then only state is updated and no API request is made to Azure.
