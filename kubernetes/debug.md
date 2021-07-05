# Know you ressources better 

you can always add yaml option at the end of the command to get the yaml description of a deployment, pod, service


    kubectl get deployments
    kubectl get svc
    kubectl get pods -o wide
    kubectl get services -o wide

# kube networking tips

    nslookup [ip]
    ping -a
    ping -a ip
    lsof -i
    nc -zv [ip]