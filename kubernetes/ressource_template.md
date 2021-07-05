# here is basic deployment definitation for a service running one container with a LoadBalancer for external access


    apiVersion : apps/v1
    kind: Deployment
    metadata:
    name: [deployment_name]
    spec:
    replicas: [replicas_amount]
    selector:
        matchLabels:
        app: [deployment_name] 
    template:
        metadata:
        labels:
            app: [deployment_name] 
        spec:
        containers:
            - name: [container_name]
            image: [container_registry_adress]
    ---
    apiVersion: v1
    kind: Service
    metadata:
        name: [service_name]
    spec:
        type: [ClusterIP/LoadBalancer/NodePort]
        ports:
        - port: 80
        selector:
            app: [deployment_name]


# Service definition : 

## ClusterIP:  The simplest type, the default one.

A ClusterIP service is the default Kubernetes service. It gives you a service inside your cluster that other apps inside your cluster can access. There is no external access.

### Why ? 
Debugging your services, or connecting to them directly from your laptop for some reason
Allowing internal traffic, displaying internal dashboards, etc.
### Why not ? 
Because this method requires you to run kubectl as an authenticated user, you should NOT use this to expose your service to the internet or use it for production services

ex:

    ---
    apiVersion: v1
    kind: Service
    metadata:
    name: "nginx-service"
    namespace: "default"
    spec:
    ports:
        - port: 80
    type: ClusterIP
    selector:
        app: "nginx"

## NodePort

A NodePort service is the most primitive way to get external traffic directly to your service. NodePort, as the name implies, opens a specific port on all the Nodes (the VMs), and any traffic that is sent to this port is forwarded to the service.


### Why ? 
Basically, a NodePort service has two differences from a normal “ClusterIP” service. First, the type is “NodePort.” There is also an additional port called the nodePort that specifies which port to open on the nodes. If you don’t specify this port, it will pick a random port. Most of the time you should let Kubernetes choose the port.
### Why not ? 
You can only have one service per port
You can only use ports 30000–32767
If your Node/VM IP address change, you need to deal with that


ex:

    ---
    apiVersion: v1
    kind: Service
    metadata:
    name: "nginx-service"
    namespace: "default"
    spec:
    ports:
        - port: 80
        nodePort: 30001
    type: NodePort
    selector:
        app: "nginx"

## LoadBalancer

A LoadBalancer service is the standard way to expose a service to the internet.

### Why ? 
If you want to directly expose a service, this is the default method. All traffic on the port you specify will be forwarded to the service. There is no filtering, no routing, etc. This means you can send almost any kind of traffic to it, like HTTP, TCP, UDP, Websockets, gRPC, or whatever.
### Why not ? 
Each exposed service has its own ip adress and we have to pay for a LoadBalancer for each exposed service -> expensive

ex:

    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
    name: my-ingress
    spec:
    backend:
        serviceName: other
        servicePort: 8080
    rules:
    - host: foo.mydomain.com
        http:
        paths:
        - backend:
            serviceName: foo
            servicePort: 8080
    - host: mydomain.com
        http:
        paths:
        - path: /bar/*
            backend:
            serviceName: bar
            servicePort: 8080