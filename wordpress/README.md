# WordPress

![Version: 0.14.0](https://img.shields.io/badge/Version-0.14.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 6.8.0-apache](https://img.shields.io/badge/AppVersion-6.8.0--apache-informational?style=flat-square)

A Helm chart for Wordpress on Kubernetes

## Introduction

This chart deploys a Wordpress instance in a Kubernetes cluster. It can be configured to use an external database or deploy its own MariaDB instance.

## Prerequisites

- Kubernetes 1.12+
- Helm 3.x
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm repo add lp_helm_charts https://linprofs.github.io/helm-charts/
helm install my-release lp_helm_charts/wordpress
```

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
helm uninstall my-release
```

## Common parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| fullnameOverride | string | `""` | Fully override the deployment name |
| nameOverride | string | `""` | Partially override the deployment name |

## Deployment parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image.pullPolicy | string | `"IfNotPresent"` | Image pull policy |
| image.registry | string | `"docker.io"` | Image registry |
| image.repository | string | `"wordpress"` | Image name |
| image.tag | string | `""` | Image tag (defaults to chart appVersion) |
| imagePullSecrets | list | `[]` | Image pull secrets |
| replicaCount | int | `1` | Number of Wordpress replicas |
| extraInitContainers | list | `[]` | Extra init containers |
| extraContainers | list | `[]` | Extra containers for usage as sidecars |
| startupProbe | object | `see values.yaml` | Startup probe configuration |
| livenessProbe | object | `see values.yaml` | Liveness probe configuration |
| readinessProbe | object | `see values.yaml` | Readiness probe configuration |
| customStartupProbe | object | `{}` | Custom startup probe (overwrites default startup probe configuration) |
| customLivenessProbe | object | `{}` | Custom liveness probe (overwrites default liveness probe configuration) |
| customReadinessProbe | object | `{}` | Custom readiness probe (overwrites default readiness probe configuration) |
| resources | object | `{}` | Resource limits and requests |
| nodeSelector | object | `{}` | Deployment node selector |
| podAnnotations | object | `{}` | Additional pod annotations |
| podSecurityContext | object | `see values.yaml` | Pod security context |
| securityContext | object | `see values.yaml` | Container security context |
| serviceAccount.create | bool | `false` | Enable service account creation |
| serviceAccount.name | string | `""` | Name of the service account |
| affinity | object | `{}` | Affinity for pod assignment |
| tolerations | list | `[]` | Tolerations for pod assignment |
| revisionHistoryLimit | int | `nil` | Maximum number of revisions maintained in revision history |

## Service parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| service.type | string | `"ClusterIP"` | Service type |
| service.port | int | `80` | Wordpress service port |
| service.nodePort | int | `nil` | The node port (only relevant for type LoadBalancer or NodePort) |
| service.clusterIP | string | `nil` | The cluster ip address (only relevant for type LoadBalancer or NodePort) |
| service.loadBalancerIP | string | `nil` | The load balancer ip address (only relevant for type LoadBalancer) |
| service.annotations | object | `{}` | Additional service annotations |

## Ingress parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| ingress.enabled | bool | `true` | Enable ingress |
| ingress.className | string | `"nginx"` | Ingress class name |
| ingress.maxBodySize | string | `"64m"` | Maximal body size |
| ingress.annotations | object | `{}` | Additional ingress annotations |
| ingress.hosts | list | `[{"host": "wordpress.openwaas.com", "paths": [{"path": "/", "pathType": "ImplementationSpecific"}]}]` | Ingress hosts |
| ingress.tls | list | `[]` | TLS settings for hosts |

## Storage parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| storage.accessModes[0] | string | `"ReadWriteOnce"` | Storage access mode |
| storage.persistentVolumeClaimName | string | `nil` | PVC name when existing storage volume should be used |
| storage.requestedSize | string | `nil` | Size for new PVC, when no existing PVC is used |
| storage.className | string | `"ebs-sc"` | Storage class name |
| storage.keepPvc | bool | `false` | Keep a created Persistent volume claim when uninstalling the helm chart |

## Wordpress parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| settings.tablePrefix | string | `nil` | Database table name prefix |
| settings.maxFileUploadSize | string | `"64M"` | Maximum file upload size |
| settings.memoryLimit | string | `"128M"` | PHP memory limit |
| settings.configExtra | string | `nil` | Extra values embedded inside wp-config.php |
| customPhpConfig | string | `nil` | Additional PHP custom.ini |
| apachePortsConfig | string | `nil` | Overwrite default apache ports.conf |
| apacheDefaultSiteConfig | string | `nil` | Overwrite default apache 000-default.conf |
| extraEnvSecrets | list | `[]` | A list of existing secrets that will be mounted into the container as environment variables |
| extraEnvConfigs | list | `[]` | A list of existing configmaps that will be mounted into the container as environment variables |
| extraSecrets | list | `[]` | A list of additional existing secrets that will be mounted into the container |

## Database parameters

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| mariadb.enabled | bool | `true` | Enable MariaDB helm chart for deployment |
| mariadb.settings.rootPassword | string | `"password"` | The root user password |
| mariadb.userDatabase.name | string | `"test"` | Name of the user database |
| mariadb.userDatabase.user | string | `"test"` | Database user with full access rights |
| mariadb.userDatabase.password | string | `"test"` | Password of the database user |
| externalDatabase.name | string | `nil` | Name of the database (is used when mariadb.enabled is false) |
| externalDatabase.user | string | `nil` | Database user (is used when mariadb.enabled is false) |
| externalDatabase.password | string | `nil` | Database password (is used when mariadb.enabled is false) |
| externalDatabase.host | string | `nil` | Database host (is used when mariadb.enabled is false) |
