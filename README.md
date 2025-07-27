# HomeHelper API Helm Chart

This Helm chart deploys the HomeHelperFinder API application on a Kubernetes cluster.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.0+
- Nginx Ingress Controller
- cert-manager (cho chứng chỉ TLS)

## Installing the Chart

To install the chart with the release name `homehelper`:

```bash
helm install homehelper ./charts/homehelper-api
```

## Uninstalling the Chart

To uninstall/delete the `homehelper` deployment:

```bash
helm uninstall homehelper
```

## Parameters

The following table lists the configurable parameters of the chart and their default values.

| Parameter                | Description             | Default        |
| ------------------------ | ----------------------- | -------------- |
| `replicaCount`           | Number of replicas      | `2`            |
| `image.repository`       | Image repository        | `homehelperapi`|
| `image.tag`              | Image tag               | `latest`       |
| `image.pullPolicy`       | Image pull policy       | `IfNotPresent` |
| `service.type`           | Service type            | `ClusterIP`    |
| `service.port`           | Service port            | `80`           |
| `service.targetPort`     | Container port          | `8080`         |
| `environment.name`       | Environment name        | `Development`  |
| `secrets.enabled`        | Enable secrets          | `true`         |
| `ingress.enabled`        | Enable ingress          | `true`         |
| `ingress.className`      | Ingress class name      | `nginx`        |
| `ingress.annotations`    | Ingress annotations     | cert-manager và health probe |
| `ingress.hosts[0].host`  | Hostname                | `api.dpercs.io.vn` |
| `ingress.tls`            | TLS configuration       | Enabled with Let's Encrypt |

## Cấu hình Ingress

Chart này bao gồm cấu hình Ingress với các tính năng sau:

- HTTPS với chứng chỉ TLS tự động từ Let's Encrypt
- Health probe path cho Azure Load Balancer
- Domain được cấu hình: `api.dpercs.io.vn`

### Yêu cầu cho TLS

Để chứng chỉ TLS được cấp chính xác:

1. cert-manager phải được cài đặt trong cluster
2. ClusterIssuer `letsencrypt-prod` phải được cấu hình
3. Domain phải được truy cập công khai để Let's Encrypt xác thực

## Configuration

You can specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

For example:

```bash
helm install homehelper ./charts/homehelper-api --set replicaCount=3
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart:

```bash
helm install homehelper ./charts/homehelper-api -f values-custom.yaml
``` 