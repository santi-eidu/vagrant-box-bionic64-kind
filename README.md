# vagrant-box-bionic64-kind
A Vagrantbox ready  for running local Kubernetes clusters with [Kind](https://kind.sigs.k8s.io/) .

## Description
This repository contains everything needed to build the bionic64-kind vagrant box.This box is based on the hashicorp/bionic64, a standard Ubuntu 18.04 LTS 64-bit provided by Hashicorp.

The next tools are included in the box:

* kind
* docker
* kubectl (with autocomplete)
* Helm

The cluster created by kind will have an ingres controller (nginx) and a local docker registry.

## Usage
You can use the base box like any other base box.

1.Prerequisites:

Install [Vagrant](https://www.vagrantup.com/docs/installation) and [Virtualbox](https://www.vagrantup.com/docs/providers/virtualbox).

2.Clone this repo:
```
$ git clone https://github.com/santi-eidu/vagrant-box-bionic64-kind.git
```

3.Create the box:
```
$ vagrant up
```

4.Login with ssh:
```
$ vagrant ssh
```

5.Get the k8s details with kubectl:

```
$ kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:36677
KubeDNS is running at https://127.0.0.1:36677/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

Ready to shine! You are ready to deploy applications on kubernetes.

Ingress example:

```
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: nginx-app.192.168.50.4.nip.io
    http:
      paths:
      - backend:
          service:
            name: nginx-service
            port:
              number: 80
        path: /
        pathType: Prefix

```
