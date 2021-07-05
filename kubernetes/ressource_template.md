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