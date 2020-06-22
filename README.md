# Ansible role - `k8s_prometheus`

Setup a Prometheus stack on K8S using the standard Helm chart `stable/prometheus`.

If you whish to setup a central Prometheus server (e.g. to aggregate metrics through a Prometheus federation), you should use the role `prometheus_server` which will bootstrap a Prometheus server in an external Docker container.

## Tags

| Tag      | Description                                |
|----------|--------------------------------------------|
| `apply`  | Deploy Prometheus and associated resources |
| `delete` | Remove Prometheus and associated resources |

## Variables

| Variable                          | Type     | Required | Default       | Description                                                |
|-----------------------------------|----------|----------|---------------|------------------------------------------------------------|
| `k8s_prometheus_namespace`        |          |          |               |                                                            |
| `k8s_prometheus_server_prefixURL` | `string` | No       | `/prometheus` | Prometheus URL prefix                                      |
| `k8s_prometheus_release`          | `string` | No       | `prometheus`  | Helm release name                                          |
| `k8s_prometheus_custom_manifests` | `list`   | No       | `[]`          | Custom manifests to apply; must be located in `templates/` |

## Templates

The following templates are the *custom manifests* which can be applied when setting the variable `k8s_prometheus_custom_manifests`. They are used to help the integration of the Prometheus deployment in the current Kubernetes infrastructure, i.e. by providing support for you CRDs (like Istio ones).

| Template   | Description            |
|------------|------------------------|
| `istio-vs` | Istio `VirtualService` |

### Template - `istio-vs`

Apply or delete an Istio `VirtualService` allowing access to Prometheus. This manifest template support the following variables:

| Variable                                   | Type     | Required | Default                      | Description                    |
|--------------------------------------------|----------|----------|------------------------------|--------------------------------|
| `k8s_prometheus_custom_istio_vs_name`      | `string` | No       | `prometheus-vs`              | The `VirtualService` name      |
| `k8s_prometheus_custom_istio_vs_namespace` | `string` | No       | `default`                    | The `VirtualService` namespace |
| `k8s_prometheus_custom_istio_vs_hosts`     | `list`   | No       | `["*"]` (all hosts)          | Allowed hosts                  |
| `k8s_prometheus_custom_istio_vs_gateway`   | `string` | No       | `istio-system/istio-gateway` | The `VirtualService` gateway   |
| `k8s_prometheus_server_prefixURL`          | `string` | No       | `/prometheus`                | Prometheus URL prefix          |
