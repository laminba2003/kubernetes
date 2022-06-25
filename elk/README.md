# ELK Installation with Helm

## Create Fluentd ConfigMap

```bash
$ kubectl apply -f fluentd-config.yaml
```

## Install Elasticsearch

```bash
$ helm install elasticsearch bitnami/elasticsearch
```

## Install Fluentd

```bash
$ helm install fluentd --set aggregator.configMap=fluentd-elasticsearch bitnami/fluentd
```

## Install Kibana

```bash
$ helm install kibana --set elasticsearch.hosts[0]=elasticsearch bitnami/kibana
```
