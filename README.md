# `manifests`

Pre-generated Kubernetes manifests for [Bitnami](https://bitnami.com/) Helm charts.

Rather than committing to a live `helm install` at deploy time, the charts are rendered ahead of time
and the resulting YAML is stored here as plain manifests. This makes the rendered output reviewable in
pull requests and lets it be applied directly with `kubectl apply`.

## Layout

Each file is the fully rendered output of one Bitnami chart:

```text
manifests/
├── airflow.yaml
├── argo-cd.yaml
├── grafana.yaml
├── mongodb.yaml
├── postgresql.yaml
├── prometheus.yaml
└── ...
```

There are currently **103** charts rendered.

## Covered applications

The rendered charts include the usual suspects:

| Category | Applications |
| -------- | ------------ |
| Databases | `mysql`, `mariadb`, `mariadb-galera`, `mongodb`, `mongodb-sharded`, `postgresql`, `postgresql-ha`, `redis`, `redis-cluster`, `cassandra`, `influxdb`, `clickhouse`, `solr`, `memcached`, `elasticsearch` |
| Messaging | `kafka`, `rabbitmq`, `rabbitmq-cluster-operator`, `nats`, `zookeeper`, `schema-registry` |
| Observability | `prometheus`, `kube-prometheus`, `thanos`, `grafana`, `grafana-loki`, `grafana-tempo`, `grafana-operator`, `kibana`, `jaeger`, `opensearch`/`elasticsearch`, `sonarqube`, `metrics-server`, `kube-state-metrics`, `node-exporter`, `fluent-bit`, `fluentd`, `logstash`, `kubernetes-event-exporter` |
| Networking | `nginx`, `nginx-ingress-controller`, `haproxy`, `traefik` (`ingress`), `metallb`, `contour`, `consul`, `kong`, `apisix`, `oauth2-proxy`, `external-dns` |
| GitOps / CI | `argo-cd`, `argo-workflows`, `flux`, `concourse`, `jenkins`, `kubeapps` |
| App frameworks | `airflow`, `spark`, `flink`, `jupyterhub`, `mlflow`-adjacent workloads, `mxnet`, `pytorch`, `tensorflow-resnet` |
| CMS / e-commerce | `wordpress`, `drupal`, `joomla`, `magento`, `prestashop`, `opencart`, `osclass`, `suitecrm`, `ghost`, `discourse`, `mediawiki`, `moodle`, `phpbb`, `jasperreports`, `tomcat`, `wildfly`, `spring-cloud-dataflow` |
| Identity | `keycloak`, `pinniped`, `sealed-secrets`, `vault`, `kiam` |
| Storage | `minio`, `mariadb-galera`, `nfs`-adjacent storage classes |

See the `manifests/` directory for the authoritative list.

## Usage

Apply a single application:

```bash
kubectl apply -f manifests/grafana.yaml
```

Apply several:

```bash
kubectl apply -f manifests/
```

## Regenerating

The manifests are rendered with `helm template` against the Bitnami chart repository, using a
`release-name` release and the `harbor` namespace. To re-render a chart, update the chart version and
run the template command, then commit the result:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm template release-name bitnami/grafana \
  --namespace harbor \
  > manifests/grafana.yaml
```

Because the output is checked in, review the diff carefully — pinned chart versions and image tags
change over time.

## Note on namespaces

The rendered manifests target the `harbor` namespace. Adjust the namespace before applying to your
own cluster, and make sure the namespace exists:

```bash
kubectl create namespace harbor
```

## License

See the repository for licensing details.
