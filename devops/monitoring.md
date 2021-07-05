# Monitoring stuff

## Helm Grafana + prometheus in AKS

Adding and updating repo

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update

Installing prometheus

    kubectl config view --raw > ~/.kube/config
    helm install prometheus prometheus-community/prometheus \
    --namespace monitoring \
    --set alertmanager.persistentVolume.storageClass="default" \
    --set server.persistentVolume.storageClass="default"

Installing Grafana

    helm install grafana grafana/grafana \
    --namespace monitoring \
    --set persistence.storageClassName="default" \
    --set persistence.enabled=true \
    --set adminPassword=coco \
    --values datasource-grafana.yaml \
    --set service.type=LoadBalancer


grafana needs a yaml to be configured which needs to be accessible by the user executing the install, this is the datasource-grafana.yaml file

file example :

    datasources:
    datasources.yaml:
        apiVersion: 1
        datasources:
        - name: Prometheus
        type: prometheus
        url: http://prometheus-server.monitoring.svc.cluster.local
        access: proxy
        isDefault: true