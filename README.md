# Introduction

- [OpenSearch Documentation](https://opensearch.org/docs/latest)
- [OpenSearch GitHub](https://github.com/opensearch-project)


# Install

## ingress-nginx

```bash
helm install -n ingress-nginx ingress-nginx ingress-nginx --create-namespace \
--repo https://kubernetes.github.io/ingress-nginx
```

## opensearch

- [helm-charts](https://github.com/opensearch-project/helm-charts)

```bash
git clone https://github.com/opensearch-project/helm-charts.git ../opensearch-helm-charts

helm template -n opensearch opensearch ../opensearch-helm-charts/charts/opensearch -f helm-values/opensearch.yaml
helm install -n opensearch opensearch ../opensearch-helm-charts/charts/opensearch -f helm-values/opensearch.yaml --create-namespace

helm template -n opensearch opensearch-dashboard ../opensearch-helm-charts/charts/opensearch-dashboards -f helm-values/opensearch-dashboard.yaml
helm install -n opensearch opensearch-dashboard ../opensearch-helm-charts/charts/opensearch-dashboards -f helm-values/opensearch-dashboard.yaml --create-namespace
```

- [OpenSearch](http://opensearch.127.0.0.1.nip.io)
- [Dashboard](http://opensearch-dashboard.127.0.0.1.nip.io)

## filebeat-oss

- [helm-charts](https://github.com/elastic/helm-charts/tree/main/filebeat)
- [running-on-kubernetes](https://www.elastic.co/guide/en/beats/filebeat/7.10/running-on-kubernetes.html)

```bash
helm repo add elastic https://helm.elastic.co

helm template -n filebeat filebeat elastic/filebeat -f helm-values/filebeat-oss.yaml
helm install -n filebeat filebeat elastic/filebeat -f helm-values/filebeat-oss.yaml --create-namespace
```

# MEMO

## Resources

```
$ kc get po
NAME                                                         READY   STATUS    RESTARTS   AGE
opensearch-cluster-master-0                                  1/1     Running   0          26m
opensearch-cluster-master-1                                  1/1     Running   0          27m
opensearch-cluster-master-2                                  1/1     Running   0          27m
opensearch-dashboard-opensearch-dashboards-cdc49c74c-xkn9g   1/1     Running   0          75m

$ kc get po -n filebeat
NAME                      READY   STATUS    RESTARTS   AGE
filebeat-filebeat-2k8z2   1/1     Running   0          25m

$ kc get ing
NAME                                         CLASS   HOSTS                                   ADDRESS     PORTS   AGE
opensearch-cluster-master                    nginx   opensearch.127.0.0.1.nip.io             localhost   80      86m
opensearch-dashboard-opensearch-dashboards   nginx   opensearch-dashboard.127.0.0.1.nip.io   localhost   80      77m
```

## override_main_response_version

- [Agents and ingestion tools](https://opensearch.org/docs/latest/clients/agents-and-ingestion-tools/index/)

before

```json
{
  "name" : "opensearch-cluster-master-0",
  "cluster_name" : "opensearch-cluster",
  "cluster_uuid" : "2zF1nQWlREy4mjyjqZywdQ",
  "version" : {
    "distribution" : "opensearch",
    "number" : "1.3.1",
    "build_type" : "tar",
    "build_hash" : "c4c0672877bf0f787ca857c7c37b775967f93d81",
    "build_date" : "2022-03-29T18:35:30.372094Z",
    "build_snapshot" : false,
    "lucene_version" : "8.10.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "The OpenSearch Project: https://opensearch.org/"
}
```

after

```json
{
  "name" : "opensearch-cluster-master-0",
  "cluster_name" : "opensearch-cluster",
  "cluster_uuid" : "2zF1nQWlREy4mjyjqZywdQ",
  "version" : {
    "number" : "7.10.2",
    "build_type" : "tar",
    "build_hash" : "c4c0672877bf0f787ca857c7c37b775967f93d81",
    "build_date" : "2022-03-29T18:35:30.372094Z",
    "build_snapshot" : false,
    "lucene_version" : "8.10.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "The OpenSearch Project: https://opensearch.org/"
}
```
