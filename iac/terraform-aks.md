# Deploy AKS with container registry and rights to it with terraform


how to init in the Azure cloud 

    terraform init -backend-config="backend.tfvars"

how to apply

    terraform apply

How to import existing ressources

    terraform [global options] import [options] ADDR ID

main.tf

    terraform {
    backend "azurerm" {}
    }

    provider "azurerm" {
    features {}
    }

    resource "azurerm_resource_group" "hogwarts" {
    name     = "Hogwarts"
    location = "West Europe"
    }

    resource "azurerm_kubernetes_cluster" "olivanders" {
    name                = "Olivanders"
    location            = azurerm_resource_group.hogwarts.location
    resource_group_name = azurerm_resource_group.hogwarts.name
    dns_prefix          = "olivandersask"

    node_resource_group = "olivanode"

    default_node_pool {
        name       = "default"
        node_count = 1
        vm_size    = "Standard_D2_v2"
        availability_zones =["1"]
    }

    role_based_access_control {
        enabled = true
    }

    identity {
        type = "SystemAssigned"
    }

    tags = {
        Environment = "dev"
    }
    }

    resource "azurerm_container_registry" "spellContainerRegistry" {
    name                = "SpellContainerRegistry"
    resource_group_name = azurerm_resource_group.hogwarts.name
    location            = azurerm_resource_group.hogwarts.location
    sku                 = "Standard"
    admin_enabled       = true

    tags = {
        environment = "dev"
    }
    }

    # add the role to the identity the kubernetes cluster was assigned
    resource "azurerm_role_assignment" "olivanders_to_spellContainerRegistry" {
    scope                = azurerm_container_registry.spellContainerRegistry.id
    role_definition_name = "AcrPull"
    principal_id         = azurerm_kubernetes_cluster.olivanders.kubelet_identity[0].object_id
    }

output.tf

    resource "local_file" "kubeconfig" {
    depends_on   = [azurerm_kubernetes_cluster.olivanders]
    filename     = "kubeconfig"
    content      = azurerm_kubernetes_cluster.olivanders.kube_config_raw
    }
