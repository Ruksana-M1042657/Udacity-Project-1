# Deploying a web server in Azure

## Introduction

Your company's development team has created an application that they need deployed to Azure. The application is self-contained, but they need the infrastructure to deploy it in a customizable way based on specifications provided at build time, with an eye toward scaling the application for use in a CI/CD pipeline.

Although we’d like to use Azure App Service, management has told us that the cost is too high for a PaaS like that and wants us to deploy it as pure IaaS so we can control cost. Since they expect this to be a popular service, it should be deployed across multiple virtual machines.

To support this need and minimize future work, we will use Packer to create a server image, and Terraform to create a template for deploying a scalable cluster of servers—with a load balancer to manage the incoming traffic. We’ll also need to adhere to security practices and ensure that our infrastructure is secure.

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

  refer "packer_build_completion.jpg"

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
