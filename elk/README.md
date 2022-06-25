# ELK Installation with Helm

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
