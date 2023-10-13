
# Neoload Web dynamic infrastructure user access

# Introduction

This chart deploys user credentials to use Neoload Web Dynamic infrastructure on a Kubernetes cluster.
In the [details chapter](#details) you can have an overview of every objects created in the cluster.

## Prerequisites

- A running [Kubernetes](https://kubernetes.io/) cluster (1.9.0 - 1.27.0)
- [Helm](https://helm.sh/docs/intro/install/) CLI  (3.2+)


## Installation

1. Add the Neotys chart repository or update it if you already had it registered

```bash		
helm repo add neotys https://helm.prod.neotys.com/stable/
```

```bash		
helm repo update
```

2. Download and set up your **[values-custom.yaml](/values-custom.yaml)** file

```bash
wget https://raw.githubusercontent.com/Neotys-Labs/helm-dynamic-infrastructure/master/values-custom.yaml
``` 
> You can refer to the ['Getting started'](#Configuration) section for basic configuration options.
>
> You can skip this step if you have nothing to change in the configuration.

3. Create a dedicated namespace

```bash		
kubectl create namespace my-namespace
```

4. Install with the following command

```bash		
helm install my-release neotys/nlweb-dynamic-infrastructure -n my-namespace -f ./values-custom.yaml
```

> Since Helm 3.2+ you can skip step 3, and add the --create-namespace option to this command
> 
> If you do not uses custom values you can remove "-f ./values-custom.yaml" from the command.

## Uninstall

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
 1. A role binding called `my-release-rolebinding`
 1. A service account called `my-release-sa`

