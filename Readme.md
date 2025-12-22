
# Neoload Web dynamic infrastructure user access

## Introduction

This Helm chart enables performance engineers to automatically deploy load testing infrastructure on a Kubernetes cluster through NeoLoad Web.
It provisions the required user credentials and deploys an agent that connects your cluster to NeoLoad Web's Dynamic Infrastructure feature.

For a complete overview of all resources created in your cluster, see the [Details](#details) section.

> [!IMPORTANT]
> Two versions of this chart are available:
>
> - **v1.x (Latest)** — Deploys an agent that communicates with NeoLoad Web. This version is more secure as it uses outbound-only connections. **Recommended for environments with strict security requirements.**
> - **v0.x** — Requires direct inbound API access to the cluster from NeoLoad Web. A simpler setup that works well when inbound access is acceptable. To migrate from v0.x to v1.x, see the [Migration guide](doc/MIGRATION_GUIDE_V0_TO_V1.md). To install v0.x, see the [v0.x Readme](https://github.com/Neotys-Labs/helm-dynamic-infrastructure/blob/stable/v0_x/Readme.md).

## Prerequisites

- A running [Kubernetes](https://kubernetes.io/) cluster (1.9.0 - 1.30.0)
- [Helm](https://helm.sh/docs/intro/install/) CLI  (3.2+)


## Installation

#### 1. Add the Neotys chart repository or update it if you already had it registered

```bash		
helm repo add neotys https://helm.prod.neotys.com/stable/
```

```bash		
helm repo update
```

#### 2. Download and set up your **[values-custom.yaml](/values-custom.yaml)** file

```bash
wget https://raw.githubusercontent.com/Neotys-Labs/helm-dynamic-infrastructure/master/values-custom.yaml
``` 
Refer to the [Configuration](#Configuration) section for configuration options.

#### 3. Install

```bash		
helm install my-release neotys/nlweb-dynamic-infrastructure --create-namespace -n my-namespace -f ./values-custom.yaml
```

## Uninstall

To uninstall the `my-release` deployment:

```bash
$ helm uninstall my-release -n my-namespace
```

## Configuration

Update `values-custom.yaml` file according to your needs.

### Agent configuration

| Parameter                  | Description                                                                                                                                                                                                                                      | Default                                                     | Required |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------|:--------:|
| `agent.name`               | Agent friendly name that will be displayed in NeoLoad Web                                                                                                                                                                                        | default name is a combination of namespace and release name |          |
| `agent.neoloadWebApiUrl`   | The URL to NeoLoad Web API (API V3, not API V4)                                                                                                                                                                                                  |                                                             |    ✅     |
| `agent.neoloadWebApiToken` | NeoLoad Web long-term token                                                                                                                                                                                                                      |                                                             |    ✅     |
| `agent.isOpenShiftCluster` | Set to `true` if current cluster is Openshift.<br/>Set to `false` if Kubernetes                                                                                                                                                                  | `false`                                                     |          |
| `agent.uuid`               | Agent UUID to uniquely identify the agent in NeoLoad Web.<br/>It is recommended not to define it in order to leave the generated value. Only useful if you want to uninstall/re-install the release and appear in NeoLoad Web as the same agent. | id is generated at installation (and kept at upgrade)       |          |

### Docker registry (optional)

Configure a private docker registry to pull the docker images of the Controller and Load generator.\
The Docker image names and pull secret are defined in NeoLoad Web's infrastructure provider.

| Parameter              | Description                               | Default |
|------------------------|-------------------------------------------|---------|
| `registryKey.enabled`  | Enable registry key to pull docker images | `false` |
| `registryKey.registry` | Docker registry URL                       |         |
| `registryKey.username` | User name of docker registry              |         |
| `registryKey.password` | Password name of docker registry          |         |

### Proxy configuration (optional)

Configure proxy settings for the agent. If enabled, you must provide an url and port.
To set credentials, recommended way would be to create a secret prior to installing this helm chart, and referring to it via secret name and key. 
You can pass it as values otherwise.

| Parameter                   | Description                                                         | Default |  Required  |
|-----------------------------|---------------------------------------------------------------------|---------|:----------:|
| `proxy.enabled`             | Enable proxy configuration for the agent                            | `false` |            |
| `proxy.url`                 | Proxy server URL (e.g., `proxy.example.com`)                        |         | if enabled |
| `proxy.port`                | Proxy server port (e.g., `8080`)                                    |         | if enabled |
| `proxy.user`                | Proxy username in plain text.                                       |         |            |
| `proxy.userSecret.name`     | Name of an existing Kubernetes secret containing the proxy username |         |            |
| `proxy.userSecret.key`      | Key in the secret containing the proxy username                     |         |            |
| `proxy.password`            | Proxy password in plain text.                                       |         |            |
| `proxy.passwordSecret.name` | Name of an existing Kubernetes secret containing the proxy password |         |            |
| `proxy.passwordSecret.key`  | Key in the secret containing the proxy password                     |         |            |


## Details

This chart creates:
 1. A namespace called `my-namespace`
 2. A role called `my-release-role` with the following rules:
    ``` yaml
    rules:
    - apiGroups: [ "apps", "extensions" ]
      resources: ["deployments", "replicasets"]
      verbs: ["get", "list", "create", "update", "patch", "delete"]
    - apiGroups: [ "" ]
      resources: ["pods", "events"]
      verbs: ["get", "list"]
    - apiGroups: [ "" ]
      resources: ["replicationcontrollers"]
      verbs: ["get", "list", "create", "update", "patch", "delete"]
    ```
 3. A role binding called `my-release-rolebinding`
 4. A service account called `my-release-sa`
 5. A config map for agent environment variables called `my-release-nlweb-dynamic-infrastructure-agent-config`
 6. A secret to store NeoLoad Web long-term token `my-release-nlweb-dynamic-infrastructure-nlweb-secrets`
 7. A deployment for the agent pod called `my-release-nlweb-dynamic-infrastructure-agent`

