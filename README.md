# Helm Installation

## Create Fluentd ConfigMap

```bash
$ kubectl apply -f fluentd-config.yaml
```

## Install Elasticsearch

```bash
$ helm install elasticsearch bitnami/elasticsearch
```

Elasticsearch can be accessed within the cluster on port 9200 at elasticsearch.default.svc.cluster.local

To access from outside the cluster execute the following commands:

```bash
kubectl port-forward --namespace default svc/elasticsearch 9200:9200
curl http://127.0.0.1:9200/
```

## Install Fluentd

```bash
$ helm install fluentd --set aggregator.configMap=fluentd-elasticsearch bitnami/fluentd
```

## Install Kibana

```bash
$ helm install kibana --set elasticsearch.hosts[0]=elasticsearch bitnami/kibana
```

Kibana can be accessed within the cluster on port 9200 at kibana.default.svc.cluster.local

To access from outside the cluster execute the following commands:

```bash
kubectl port-forward --namespace default svc/kibana 5601:5601
curl http://127.0.0.1:5601/
```

## Install Prometheus Operator

```bash
$ helm install kibana --set elasticsearch.hosts[0]=elasticsearch bitnami/kibana
```

Watch the Prometheus Operator Deployment status using the command:

```bash
kubectl get deploy -w --namespace default -l app.kubernetes.io/name=kube-prometheus-operator,app.kubernetes.io/instance=promotheus
kubectl get sts -w --namespace default -l app.kubernetes.io/name=kube-prometheus-prometheus,app.kubernetes.io/instance=promotheus
```

Prometheus can be accessed via port "9090" on the following DNS name from within your cluster: promotheus-kube-prometheus-prometheus.default.svc.cluster.local

To access Prometheus from outside the cluster execute the following commands:

```bash
kubectl port-forward --namespace default svc/promotheus-kube-prometheus-prometheus 9090:9090
curl http://127.0.0.1:9090/
```

Watch the Alertmanager StatefulSet status using the command:

```bash
kubectl get sts -w --namespace default -l app.kubernetes.io/name=kube-prometheus-alertmanager,app.kubernetes.io/instance=promotheus
```

Alertmanager can be accessed via port "9093" on the following DNS name from within your cluster: promotheus-kube-prometheus-alertmanager.default.svc.cluster.local

To access Alertmanager from outside the cluster execute the following commands:

```bash
kubectl port-forward --namespace default svc/promotheus-kube-prometheus-alertmanager 9093:9093
curl http://127.0.0.1:9093/
```

## Install Grafana

```bash
helm install grafana bitnami/grafana
```

To access Kibana from outside the cluster execute the following commands:

```bash
kubectl port-forward svc/grafana 8080:3000
curl http://127.0.0.1:8080
```

To get the admin credentials execute the following commands:

```bash
echo "User: admin"
echo "Password: $(kubectl get secret grafana-admin --namespace default -o jsonpath="{.data.GF_SECURITY_ADMIN_PASSWORD}" | base64 -d)"
```
