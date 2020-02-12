# Neoload Web dynamic infrastructure user

# Introduction

This chart deploys user credentials to use Neoload Web Dynamic infrastructure on a kubernetes cluster.


## Prerequisites

- A running [Kubernetes](https://kubernetes.io/) cluster (1.9+)
- [Helm](https://helm.sh/docs/intro/install/) CLI  (~3.0.2)


## Installation

1. Download and set up your **[values-custom.yaml](/nlweb/values-custom.yaml)** file
2. Install with the following command

```bash		
helm repo add https://sylvain-brun-neotys.github.io/helm 
helm install my-release neotys-chart/nlweb-dynamic-infrastructure-user
```

## Uninstalling the Chart

To uninstall the `my-release` deployment:

```bash
$ helm uninstall my-release
```

## Configuration

Parameter | Description | Default
----- | ----------- | -------
`registryKey.enabled` | Enable registry key to pull docker images | `false`
`registryKey.registry` | Docker registry URL |
`registryKey.username` | User name of docker registry |
`registryKey.password` | Password name of docker registry |

