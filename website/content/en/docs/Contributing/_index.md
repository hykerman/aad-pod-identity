---
title: "Contributing"
linkTitle: "Contributing"
weight: 7
date: 2020-10-04
description: >
  The AAD Pod Identity project welcomes contributions and suggestions.
---

## CLA

The AAD Pod Identity project welcomes contributions and suggestions. Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit [https://cla.microsoft.com](https://cla.microsoft.com).

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

## Development

### Prerequisites

- Go 1.17
- Docker
- an AKS cluster / AKS-Engine cluster
- Helm 3
- A container registry. One of the following would work:
  - Dockerhub
  - Azure Container Registry (ACR)

### Development flow

```bash
git clone https://github.com/Azure/aad-pod-identity ${GOPATH}/src/github.com/Azure/aad-pod-identity
cd ${GOPATH}/src/github.com/Azure/aad-pod-identity

export REGISTRY=<RegistryName>

# Build MIC and NMI images
make image-mic image-nmi

# push images to your container registry
make push-mic push-nmi

# install the above images on your cluster
helm install aad-pod-identity manifest_staging/charts/aad-pod-identity \
  --set image.repository="${REGISTRY}" \
  --set mic.tag=v0.0.0-dev \
  --set nmi.tag=v0.0.0-dev
```

## Test Standard

### Unit tests

Unit tests focus on testing individual units and components of aad-pod-identity and they don't require any additional services to execute. They should be fast and great for getting the first signal on whether the implementation works or not. To run unit tests:

```bash
make unit-test
```

In most cases, pull requests should include an adequate amount of unit tests such that the code coverage of the pull request may not lessen the code coverage of the  main branch.

### End-to-end tests

End-to-end testing tests whether the flow of aad-pod-identity from start to finish is behaving as expected. Following guidelines should be followed when developing E2E tests:

- Use the aad-pod-identity e2e test framework.

- Define test spec reflecting real user workflow, e.g. [creation of AzureIdentity, AzureIdentityBinding and a pod that consumes the identity](https://github.com/Azure/aad-pod-identity/blob/5c9c5e541d6612c31af4d09dc0ec7654388cc076/test/e2e/single_identity_test.go#L33-L96).

See [test/e2e/README.md](https://github.com/Azure/aad-pod-identity/blob/master/test/e2e/README.md) on how to run the aad-pod-identity e2e test suite locally.
