
# Neoload Web dynamic infrastructure user access

# Introduction

This chart deploys user credentials to use Neoload Web Dynamic infrastructure on a Kubernetes cluster.
In the [details chapter](#details) you can have an overview of every objects created in the cluster.

## Prerequisites

- A running [Kubernetes](https://kubernetes.io/) cluster (1.9.0 - 1.18.0)
- [Helm](https://helm.sh/docs/intro/install/) CLI  (3.2+)


## Installation

1. Add the Neotys chart repository or update it if you already had it registered

```bash		
helm repo add neotys https://helm.prod.neotys.com/stable/
```

```bash		
helm repo update
```

2. Install with the following command

```bash		
helm install my-release neotys/nlweb-dynamic-infrastructure -n my-namespace --create-namespace
```

## Uninstalling the Chart

To uninstall the `my-release` deployment:

```bash
$ helm uninstall my-release -n my-namespace
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
 1. A namespace called `my-namespace`
 1. A role called `my-release-role` with the following rules:
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
 1. A role binding called `myrelease-rolebinding`
 1. A service account called `myrelease-sa`

