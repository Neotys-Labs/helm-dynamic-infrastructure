# Neoload Web Dynamic Infrastructure User Access

## Introduction

This chart deploys the credentials required for NeoLoad Web Dynamic Infrastructure on a Kubernetes cluster.  
In the [Details](#details) section you can find an overview of all objects created in the cluster.  

> **Security note:** This chart supports using Kubernetes `imagePullSecrets` for secure access to private registries.  
> The previous method of passing credentials via the `registryKey` field in `values.yaml` is still supported but **deprecated**.

## Prerequisites

- A running [Kubernetes](https://kubernetes.io/) cluster (v1.9.0–v1.30.0)  
- [Helm](https://helm.sh/docs/intro/install/) CLI (v3.2+)

## Installation

1. **Add (or update) the Neotys Helm repo**

```bash		
helm repo add neotys https://helm.prod.neotys.com/stable/
helm repo update
```

2. **(if you need to pull private images) Create your registry pull secret (optional!)**

```bash
kubectl create secret docker-registry my-registry-secret \
  --docker-server=REGISTRY_URL \
  --docker-username=USERNAME \
  --docker-password=PASSWORD \
  --namespace=my-namespace
``` 
> Replace REGISTRY_URL, USERNAME, PASSWORD and the secret name as needed.
> If you manage secrets via GitOps, you can instead commit a Secret manifest to your repo.

3. **(if you need to pull private images) Download and customize your values file (optional!)**

```yml
imagePullSecrets:
  - name: my-registry-secret
```

5. **Create a dedicated namespace**

```bash		
kubectl create namespace my-namespace
```
> Or just use --create-namespace in the next step.

4. **Install (or upgrade) the chart**

```bash		
helm install my-release neotys/nlweb-dynamic-infrastructure \
  -n my-namespace \
  --create-namespace \
  -f values-custom.yaml
```
> If you do not uses custom values you can remove "-f ./values-custom.yaml" from the command.

## Uninstall

To remove the release:

```bash
$ helm uninstall my-release -n my-namespace
```

## Configuration

Parameter | Description | Default
--------- | ----------- | -------
`registryKey.enabled` | *(Deprecated)* Enable registry key to pull docker images | `false`
`registryKey.registry` | *(Deprecated)* Docker registry URL | `""`
`registryKey.username` | *(Deprecated)* Username for Docker registry | `""`
`registryKey.password` | *(Deprecated)* Password for Docker registry | `""`
`imagePullSecrets` | A list of existing Kubernetes secrets used to pull private images | `[]`


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

## Deprecation Notice

The `registryKey` field used to configure Docker registry credentials directly in `values-custom.yaml` is now deprecated.

Instead, we recommend:

 1. Creating a Kubernetes pull secret manually:

```bash
kubectl create secret docker-registry my-registry-secret \
  --docker-server=REGISTRY_URL \
  --docker-username=USERNAME \
  --docker-password=PASSWORD \
  --namespace=my-namespace
```

 2. Referencing it in your `values-custom.yaml`:

``` yaml
imagePullSecrets:
  - name: my-registry-secret
```
