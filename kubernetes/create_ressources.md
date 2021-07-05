# create deployment

    kubectl create deployment [deployment name] --image=[image adress name in the contianer registry]

# create service for deployment 

default is a clusterIp only accessible from the cluster (other containers)

    kubectl expose deployment [deployment name] --port [port] 

you can specify a service type 

    kubectl expose deployment [deployment name] --port [port] --type=LoadBalancer

# create namespace

    kubectl create namespace [namespace name]