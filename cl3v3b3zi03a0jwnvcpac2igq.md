---
title: "ValidKube : Securing your YAML"
seoTitle: "ValidKube : Securing your YAML"
seoDescription: "Validate, Clean, Secure and Audit your Kubernetes Manifest files using ValidKube, as OSS powered by Komodor."
datePublished: Wed Jun 01 2022 04:30:01 GMT+0000 (Coordinated Universal Time)
cuid: cl3v3b3zi03a0jwnvcpac2igq
slug: validkube-securing-your-yaml
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654066415052/xngmP7vgz.jpg
tags: kubernetes, yaml, k8s

---

## Introduction

[Komodor](https://komodor.com/) is the kubernetes troubleshooting platform that monitors your entire k8s stack, identifies issues, helps in uncovering their root cause and delivers the context you need to troubleshoot efficiently and independently. 

And [ValidKube](https://validkube.com/) is the OSS is a simple web tool that combines a few other OSS tools which allows quick scanning of YAMLs for hygiene, security and validity, it's made and maintained by the Komodor. 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653827184637/eV9ZXSFWm.png align="left")

## What ValidKube does?

In the last few years, there are many companies that are focusing more on the development part of DevOps making the work of developers easy, **ValidKube **being one of tools that is focused on making the developers work and experience both smooth and easy. Working with Kubernetes and YAML files as the beginner is really difficult sometimes, as you have to debug and making sure that clusters are secure. And this is where ValidKube comes in, it simplifies the developers kubernetes deployments, since it's and online platform so no kind of installations are required.

It can be just run from your browser, and it will validate & clean the files for you.

## Features  

As we discussed, ValidKube project uses other opensource projects as it's part to provide best features. It has following capabilities:-

- **Validate **- Verify your Kubernetes configuration files using [@kubeval](https://github.com/instrumenta/kubeval)
- **Clean **- Remove clutter from your Kubernetes manifests using [@kubectl-neat](https://github.com/itaysk/kubectl-neat)
- **Secure **- Scan your YAML code for security vulnerabilities using [@trivy](https://github.com/aquasecurity/trivy)
- **Audit **-Validation of best practices for your yaml using [@polaris](https://github.com/FairwindsOps/polaris)

### How it works?

We will be using YAML file mentioned below:-
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: example
  labels:
    app: nginx
spec:
  replicas: "Wrong"
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        args: []
        ports:
        - containerPort: 80
        resources: {}
```

Once the sample YAML is up, click on run and it will validate whether the YAML file is correct or not.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653828754379/K60gSGM-0.png align="left")

It validates and throw an error, which is correct as we can see ``spec.replicas`` is in the form of the string and we know that's the wrong format,  it can either be in the form of ``Integer`` or ``Null``. Now let's change it value to integer and see what happens:

```yaml
spec:
  replicas: 1
```
This time we got no errors and status is YAML file is valid, implying that it is fine.
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653829063193/z_J7XbZSQ.png align="left")

Similarly, While running clean on the file it makes the file more neat and clean and readable for everyone, and when we run secure on the file we can check how many exceptions, failures and successes have occurred, as we have seen it uses the trivy for that purpose, so you will get information about how misconfigs and failures could be fixed. And the last service that it currently provides it Audit, it can help you to know more about your k8s yaml such has when it was created, clusters information, results such as ``cpuLimits``, ``cpuRequests`` and more.

---
## Follow Up Resources:-

1. ValidKube: https://validkube.com/
2. GitHub: https://github.com/komodorio/validkube
3. Komodor: https://komodor.com/

You can checkout their GitHub and star it, since it's OSS you can work on current open issues or add more tools or capabilities to it. Give Komodor a follow on twitter, all the updates and news are released there.


