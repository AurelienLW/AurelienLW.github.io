# Deploy AKS with container registry and rights to it with terraform


how to init in the Azure cloud :

You need to create an account storage and then a container in it and use this container as a backend

You can give access to it with a backend.tfvars file

    resource_group_name= "iaac"
    storage_account_name= "wands"
    container_name= "bigwand"
    key= "terraform.tfstate"


the command to setup the backend is as follow

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

    resource "azurerm_resource_group" "aurel-rg" {
    name     = "aurelien-aks-dockercoins"
    location = "West Europe"
    }

    resource "azurerm_kubernetes_cluster" "aurelAks" {
    name                = "AurelAks"
    location            = azurerm_resource_group.aurel-rg.location
    resource_group_name = azurerm_resource_group.aurel-rg.name
    dns_prefix          = "aurelDnsAks"

    node_resource_group = "AurelNRGAks"

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

output.tf

    resource "local_file" "kubeconfig" {
    depends_on   = [azurerm_kubernetes_cluster.aurelAks]
    filename     = "kubeconfig"
    content      = azurerm_kubernetes_cluster.aurelAks.kube_config_raw
    }
    output "kube_config" {
    value = azurerm_kubernetes_cluster.aurelAks.kube_config_raw
    sensitive = true
    }

    output "resource_group_name" {
    value = azurerm_kubernetes_cluster.aurelAks.resource_group_name
    }

    output "cluster_name" {
    value = azurerm_kubernetes_cluster.aurelAks.name
    }
