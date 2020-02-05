# Neoload Web

[Neoload Web](https://www.neotys.com/neoload/overview) is a performance testing software for all team members from Centers of Excellence to DevOps organizations.

## Introduction

This chart deploys Neoload Web on your kubernetes cluster.

This chart is meant for advanced kubernetes/helm users as a successful installation and exploitation of the application is very environment dependant.

## Prerequisites

- A running [Kubernetes](https://kubernetes.io/) cluster (1.14+)
- [Helm](https://helm.sh/docs/intro/install/) CLI  ~3.0.2
- A running [mongodb](https://www.mongodb.com/) accessible from the kubernetes cluster
- One of the following ingress controller installed on your kubernetes cluster

#### Ingress controller

There are many different ingress controller providers.

This chart is shipped with sensible values for both [**nginx**](https://hub.helm.sh/charts/bitnami/nginx) and [**alb**](https://hub.helm.sh/charts/incubator/aws-alb-ingress-controller).

> **Caution**: Selecting any other ingress controllers will require additional chart tuning from your part.
	
## Installation

1. Download and set up your **[values-custom.yaml]()** file
2. Install with the following command

```bash		
helm install my-release stable/nlweb -f ./values-custom.yaml
```

## Uninstalling the Chart

To uninstall the `my-release` deployment:

```bash
$ helm uninstall my-release
```

## Configuration

Value | Description | Default
----- | ----------- | -------
ABC | DEF | GHI

## TLS

To enable TLS and access Neoload Web via https, the parameters :

- `ingress.enabled` must be true
- `ingress.tls` must contain at least one item with the tls secret data

> **Caution**: Ingresses support multiple TLS mapped to respective hosts and paths. This feature is not supported for Neoload Web, i.e. exactly zero or one TLS configuration is expected.

### Using an existing tls secret

Simply refer to your secret in the `ingress.tls[0].secretName` parameter, and leave both `ingress.tls[0].secretCertificate` and `ingress.tls[0].secretKey` empty.

### Creating a new tls secret

##### Provide a certificate and a private key

Use the following documentation or use your own means to provide both a certificate and a private key.

- [Kubernetes TLS Secret generation documentation](https://kubernetes.github.io/ingress-nginx/user-guide/tls/)

##### Add these to your custom values file

Copy the content of the files into the `ingress.tls[0].secretCertificate` and `ingress.tls[0].secretKey` parameters.

##### Specify your new tls secret name

Set a name for your new tls secret name into the `ingress.tls[0].secretName` parameter.


## Neoload Web components overview

This section is designed to help you gain a better understanding of the many components at work in Neoload Web.