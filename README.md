# Azure Infrastructure Operations: Deploying a scalable IaaS web server in Azure

## Introduction
The goal of this project is to create infrastructure as code (IaC) in the form of a Terraform template as well as a Packer configuration to deploy a highly available website with a load balancer, as shown in the diagram below. The infrastructure is deployed into Azure in a customizable way based on specifications provided at build time, with an eye toward scaling the application for use in a CI/CD pipeline.

Although we could use Azure App Service to achieve the same goal, we have adopted this approach in order to make the project suitable for use in an environment or organisation where the cost 
of PaaS is a concern. Therefore, we have used only Azure IaaS so we can control cost. Since we expect the application be a popular service, it was deployed across multiple virtual machines.

To support this need and minimize future work, we employed Packer to create a server image, and Terraform to create a template for deploying a scalable cluster of servers with a load balancer to manage the incoming traffic. We have also adhered to security practices and ensured that our infrastructure is secure.

### Main Steps
The project consist of the following main steps:

-   Creating a Packer template
-   Creating a Terraform template
-   Deploying the infrastructure

## Getting Started

  ### Prepare the dependencies:

  1. Create an [Azure Account](https://portal.azure.com) 
  2. Install the [Azure Client/Command-Line Interface (CLI)](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
  3. Install [Packer](https://www.packer.io/downloads)
  4. Install [Terraform](https://www.terraform.io/downloads.html)

  ### Instructions

  Packer Creation:
  
  - Create a resource using command "az group create -n myResourceGroup -l westus"
  - Fetch client ID, Client_secret_ID, tenant_ID by creating service principle using "az ad sp create-for-rbac --role Contributor --scopes /subscriptions/<subscription_id> --query "{ client_id: appId, client_secret: password, tenant_id: tenant }""
  - Define packer template as per server.json file using the above fetched IDs

  Build the project following the guide provided in the links below:

  - Run "packer build server.json" to build the packer
  - Run "terraform plan -out solution.plan" to deploy the image using terraform and to save the output in solution.plan
  - Run "terraform destroy" to delete the infrastructure created  
    
### Output

1. Packer build output should be similar to this:

  ```bash
  ==> azure-arm: Cleanup requested, deleting resource group ...
  ==> azure-arm: Resource group has been deleted.
  Build 'azure-arm' finished after 1 hour 19 minutes.

  ==> Wait completed after 1 hour 19 minutes

  ==> Builds finished. The artifacts of successful builds are:
  --> azure-arm: Azure.ResourceManagement.VMImage:

  OSType: Linux
  ManagedImageResourceGroupName: packer-images-rg
  ManagedImageName: UbuntuServerPackerImage
  ManagedImageId: /subscriptions/02dbcxxxx-xxxxxxxxx-xxxxxx-xxxxx/resourceGroups/packer-images-rg/providers/Microsoft.Compute/images/UbuntuServerPackerImage
  ManagedImageLocation: West US 2
  ```

2. Terraform: 
 
  Terraform 0.11 and earlier required all non-constant expressions to be
  provided via interpolation syntax, but this pattern is now deprecated. To
  silence this warning, remove the "${ sequence from the start and the }"
  sequence from the end of this expression, leaving just the inner expression.

  Template interpolation syntax is still used to construct strings from
  expressions when the template includes multiple interpolation sequences or a
  mixture of literal strings and interpolations. This deprecation applies only
  to templates that consist entirely of a single interpolation sequence.


  Apply complete! Resources: 15 added, 0 changed, 0 destroyed.

  The state of your infrastructure has been saved to the path
  below. This state is required to modify and destroy your
  infrastructure, so keep it safe. To inspect the complete state
  use the `terraform show` command.

  State path: terraform.tfstate


## References

- Microsoft 2021, accessed 2021-01-23,\
  <https://microsoft.github.io/AzureTipsAndTricks/blog/tip201.html>
- Microsoft 2021, accessed 2021-01-23,\
  <https://docs.microsoft.com/en-us/azure/virtual-machines/linux/build-image-with-packer>