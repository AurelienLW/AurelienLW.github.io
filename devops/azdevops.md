# Azure devops stuff

## Kube Apply to AKS (Azure kubernetes service)

To let Azure Devops execute a Kubectl apply in your aks you need to have a service connection ready to let azure devops authent to your AKS cluster

Project settings -> Service Connection -> New Service connection -> Kubernetes -> Subscribtion -> Cluster -> Name of the Service Connection

Creating the pipeline you can specify your git repo where you want your pipeline yaml to be hosted

Here an example of a Azure devops Pipeline doing a kubectl apply on a /kube folder 

    trigger:
    - master

    pool:
    vmImage: ubuntu-latest

    steps:
    - task: Kubernetes@1
    inputs:
        connectionType: 'Kubernetes Service Connection'
        kubernetesServiceEndpoint: 'Olivanders'
        namespace: 'default'
        command: 'apply'
        arguments: '-f ./kube'
        secretType: 'dockerRegistry'
        containerRegistryType: 'Azure Container Registry'

The pipeline is triggered on a push to master
The image used is the latest ubuntu

It's using a service connection named Olivanders
Its doing A kubectl apply on the default namespace, with files present in the /kube directory. 

The AKS needs to have access to the Container registry. (RBAC)

[See in terraform deployment of the AKS how you can set it up](./iac/terraform-aks.md)


## deployment of Dockercoins with prometheus and grafana on AKS
To be working properly this pipeline needs: 
Service connection to the AKS
The aks needs access to the container registry

The pipeline needs access to the grafana values file and to the bash script setting up the repo update for the prometheus & grafana files see : [prometheus and grafana heml install](monitoring.md)

    trigger:
    - master

    pool:
    vmImage: ubuntu-latest

    steps:
    - task: Bash@3
    inputs:
        targetType: 'inline'
        script: |
        # Write your commands here
        
        echo 'Hello world'
        ls -l
    - task: Kubernetes@1
    inputs:
        connectionType: 'Kubernetes Service Connection'
        kubernetesServiceEndpoint: '[Kubernetes service Connection name]'
        namespace: 'default'
        command: 'apply'
        arguments: '-f ./kube'
        secretType: 'dockerRegistry'
        containerRegistryType: 'Azure Container Registry'
    - task: HelmInstaller@0
    inputs:
        helmVersion: '2.14.1'
        installKubectl: true

    - task: Bash@3
    inputs:
        filePath: './prometheus-grafana.sh'
    - task: HelmDeploy@0
    inputs:
        connectionType: 'Azure Resource Manager'
        azureSubscription: '[Subscriptionname]'
        azureResourceGroup: '[Ressource group name]'
        kubernetesCluster: '[Aks name]'
        namespace: 'monitoring'
        command: 'install'
        chartType: 'Name'
        chartName: 'prometheus-community/prometheus'
        releaseName: 'prometheus'
        arguments: '--namespace monitoring --set alertmanager.persistentVolume.storageClass="default" --set server.persistentVolume.storageClass="default"'

    - task: HelmDeploy@0
    inputs:
        connectionType: 'Azure Resource Manager'
        azureSubscription: '[Subscriptionname]'
        azureResourceGroup: '[Ressource group name]'
        kubernetesCluster: '[Aks name]'
        namespace: 'monitoring'
        command: 'install'
        chartType: 'Name'
        chartName: 'grafana/grafana'
        releaseName: 'grafana'
        arguments: '--set persistence.storageClassName="default" --set persistence.enabled=true --set adminPassword=''coco'' --values datasource-grafana.yaml --set service.type=LoadBalancer'