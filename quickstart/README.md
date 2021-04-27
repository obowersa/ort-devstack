TODO:
- Support PVC's which persist after a cluster delete/create
- Support different models, ingress + istio
- Detail how to use skaffold to build and deploy and debug
- Configure PVC to support persistence across cluster changes
- Add healthcheck for postgresql
- Add steps on spinning up new services, this is part of the exercises which need to be reintroduces


# Overview
For folk who are already familiar with docker, kubernetes and helm, this will quickly give you a cluster running the ortelius postgres test database.


# Requirements
You will need to install the following tools:

Docker: https://docs.docker.com/get-docker/

Kind: https://kind.sigs.k8s.io/docs/user/quick-start/

Kubectl: https://kubernetes.io/docs/tasks/tools/

Helm: https://helm.sh/

# Steps
- kind create cluster --config=./kind/config/base.yaml
```
> kind create cluster --config=./kind/config/base.yaml
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.19.1) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Thanks for using kind! 😊
>kubectl cluster-info
Kubernetes master is running at https://127.0.0.1:38839
KubeDNS is running at https://127.0.0.1:38839/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
- Deploy the ortelius devstack:
```
> kind create cluster --config=./kind/config/kind-base.yaml
```

- Once deployed, you will need to wait for the containers to come up. Postgres in paticular can take a bit of time while it does the database insert. In the logs, you will be waiting for it to report 'listening on IPV4*'  To check on this you can run:
```
>kubectl logs -f deployment/ort-postgres
ALTER TABLE
ALTER TABLE
ALTER TABLE
ALTER TABLE
ALTER TABLE
ALTER TABLE

2021-04-27 12:28:03.760 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2021-04-27 12:28:03.760 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2021-04-27 12:28:03.763 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2021-04-27 12:28:03.767 UTC [51] LOG:  database system was shut down at 2021-04-27 12:28:03 UTC
2021-04-27 12:28:03.769 UTC [1] LOG:  database system is ready to accept connections
```

- Once up and running, use the following to expose the postgres service to your localhost
```
> kubectl port-forward svc/ort-postgres 5432:5432
```

- You can now connect to postgres on localhost:5432, using un: postgres and pw: password


# Useful tools

General:
VSCode with the following extensions:

-- https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools
-- https://marketplace.visualstudio.com/items?itemName=GoogleCloudTools.cloudcode


Windows Specific:
- Windows Terminal
- WSL2
