# Terraform Azure Authentication with Service Principal and Federated Identity

This repository provides an example of how to authenticate to Azure using a service principal with federated identity while running Terraform.

## Prerequisites

- Azure subscription
- Azure CLI installed
- Terraform installed

## Steps

### 1. Create an Azure AD Application and Service Principal

First, create an Azure AD application and service principal.

```sh
az ad sp create-for-rbac --name "<service_principal_name>" --role Contributor --scopes /subscriptions/<subscription_id>

Take note of the appId, password, and tenant values from the output. These will be used as the client_id, client_secret, and tenant_id respectively.

### 2. Configure Federated Identity Credential

Navigate to the Azure AD application in the Azure portal, go to Certificates & secrets, and then to the Federated credentials tab. Click + Add Credential and configure it to trust your GitHub repository.

### 3. Set Up Terraform Configuration

Create a main.tf file with the following content:

```sh
provider "azurerm" {
  features {}

  client_id       = var.client_id
  client_secret   = var.client_secret
  tenant_id       = var.tenant_id
  subscription_id = var.subscription_id
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}
