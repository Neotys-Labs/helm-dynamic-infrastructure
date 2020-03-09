
# Neoload Web dynamic infrastructure user

# Introduction

This chart deploys user credentials to use Neoload Web Dynamic infrastructure on a kubernetes cluster.
Details chapter explain what this chart create in the Kubernetes cluster.

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

## Details

This chart create:
 1. A namespace call `my-release`
 1. A role with these rules:
	``` yaml
	rules:
	- apiGroups: [ "", "apps", "extensions" ]
	  resources: ["deployments", "replicasets"]
	  verbs: ["*"]
	- apiGroups: [ "" ]
	  resources: ["replicationcontrollers"]
	  verbs: ["*"]
	```
 1. A role binding
 1. A service account

