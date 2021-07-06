# Flux stuff


## Install Flux 

https://fluxcd.io/docs/installation/#install-the-flux-cli


    curl -s https://fluxcd.io/install.sh | sudo bash

 enable completions in ~/.bash_profile
    
    . <(flux completion bash)


## Using Flux

https://fluxcd.io/docs/use-cases/azure/


clone kube repo

Create flux files

    mkdir -p ./clusters/my-cluster/flux-system

install and generate the flux manifest

    flux install \
    --export > ./clusters/my-cluster/flux-system/gotk-components.yaml

Apply Flux component to kube 

    kubectl apply -f ./clusters/my-cluster/flux-system/gotk-components.yaml

Verify that the controllers have started:

    flux check

create git repo object on the cluster (needs to specify ssh key of the repo)
The better alternative is to create a machine-user whose sole purpose is to store credentials for automation. Using a machine-user also has the benefit of being able to be read-only or restricted to specific repositories if this is needed.

    flux create source git flux-system \
    --git-implementation=libgit2 \
    --url=ssh://git@ssh.dev.azure.com/v3/<org>/<project>/<repository> \
    --branch=<branch> \
    --ssh-key-algorithm=rsa \
    --ssh-rsa-bits=4096 \
    --interval=1m


Create a Kustomization object on your cluster:

    flux create kustomization flux-system \
    --source=flux-system \
    --path="./clusters/my-cluster" \
    --prune=true \
    --interval=10m


Export both objects, generate a kustomization.yaml, commit and push the manifests to Git:

    flux export source git flux-system \
    > ./clusters/my-cluster/flux-system/gotk-sync.yaml

    flux export kustomization flux-system \
    >> ./clusters/my-cluster/flux-system/gotk-sync.yaml

    cd ./clusters/my-cluster/flux-system && kustomize create --autodetect

    git add -A && git commit -m "add sync manifests" && git push

Wait for Flux to reconcile your previous commit with:

    watch flux get kustomization flux-system

## Flux Upgrade

To upgrade the Flux components to a newer version, download the latest flux binary, run the install command in your repository root, commit and push the changes:

    flux install \
    --export > ./clusters/my-cluster/flux-system/gotk-components.yaml

    git add -A && git commit -m "Upgrade to $(flux -v)" && git push



Flux auto image update

https://fluxcd.io/docs/guides/image-update/
https://fluxcd.io/docs/guides/image-update/#azure-container-registry
https://docs.microsoft.com/en-us/azure/aks/cluster-container-registry-integration 
