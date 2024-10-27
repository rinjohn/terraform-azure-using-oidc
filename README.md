
# Terraform Azure Authentication with Service Principal and Federated Identity

This repository provides an example of how to authenticate to Azure using a service principal with federated identity while running Terraform.

## Prerequisites

- Azure subscription
- Azure CLI installed
- Terraform installed

## Steps

### 1. Create an Azure AD Application and Service Principal

First, create an Azure AD application and service principal.
**Create an Azure AD Application**:
-   Open the Azure portal and navigate to  **Azure Active Directory**.
-   Select  **App registrations**  and click  **New registration**.
-   Enter a name for your application and click  **Register**.

### 2. Configure Federated Identity Credential

Navigate to the Azure AD application in the Azure portal, go to  **Certificates & secrets**, and then to the  **Federated credentials**  tab. Click  **+ Add Credential**  and configure it to trust your GitHub repository.

### 3. Set Up Terraform Configuration

Create a  `main.tf`  file with the following content:

```hcl
provider "azurerm" {
  features {}

  client_id       = var.client_id
  tenant_id       = var.tenant_id
  subscription_id = var.subscription_id
  use_oidc        = true
}

terraform {
  backend "azurerm" {
    storage_account_name = "your_storage_account_name"
    container_name       = "your_container_name"
    key                  = "terraform.tfstate"
    use_azuread_auth     = true
  }
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

```

### 4. Define Variables

Create a  `variables.tf`  file to define the necessary variables:

```hcl
variable "client_id" {}
variable "tenant_id" {}
variable "subscription_id" {}

```

### 5. Set Up Environment Variables

Export the necessary environment variables before running Terraform commands:

```sh
export ARM_CLIENT_ID=<your_client_id>
export ARM_TENANT_ID=<your_tenant_id>
export ARM_SUBSCRIPTION_ID=<your_subscription_id>
export ARM_USE_AZUREAD=true
```

### 6. Initialize and Apply Terraform Configuration

Run the following commands to initialize and apply the Terraform configuration:

```sh
terraform init
terraform apply
```

## Contributing

Feel free to open issues or submit pull requests if you have any improvements or suggestions.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
