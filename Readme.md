
# Neoload Web dynamic infrastructure user access

# Introduction

This chart deploys user credentials to use Neoload Web Dynamic infrastructure on a Kubernetes cluster.
In the [details chapter](#details) you can have an overview of every objects created in the cluster.

## Prerequisites

- A running [Kubernetes](https://kubernetes.io/) cluster (1.9+)
- [Helm](https://helm.sh/docs/intro/install/) CLI  (~3.0.2)


## Installation

1. Add the Neotys chart repository

```bash		
helm repo add neotys https://helm.prod.neotys.com/stable/
```

2. Install with the following command

```bash		
helm install my-release neotys/nlweb-dynamic-infrastructure
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

This chart creates:
 1. A namespace called `my-release`
 1. A role with the following rules:
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

