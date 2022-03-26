---
layout: post
comments: true
title: Deployment of the prometheus grafana suite under Kubernetes
type: post
tags: [kubernetes, prometheus, grafana, k3d, helm]
image: /images/post/2021/03/2021-03-31-deploiement-prometheus-grafana-kubernetes.png
lang: en
summary: "Supervising the cloud is a major issue when deciding to deploy a business application there, Prometheus, Grafana stand out as essential tools to meet monitoring challenges. Here's how to deploy them in a Kubernetes environment."
---

Supervising the cloud is a major issue when deciding to deploy a business application there. Indeed, what is the point of benefiting from 5*9 availability at the infrastructure level if the application layer is not monitored and does not offer the same level of continuity of service. At a time when IT architectures are evolving, when the Dev/Ops principles of CI/CD are becoming widespread, the IT department needs agile and efficient supervision tools. [Prometheus](https://go2.grafana.com/prometheus-grafana.html), [Grafana](https://go2.grafana.com/prometheus-grafana.html) stand out as essential tools to respond to these issues to provide monitoring and alerting services. Here's how to deploy them in a Kubernetes environment.

## Prometheus / Grafana : Architecture

We often talk about Prometheus and Grafana, but in reality it is a more complete tooling suite as shown in the architecture diagram below:

![Prometheus Grafana architecture](/blog/images/post/2021/03/2021-03-31-deployment-prometheus-grafana-kubernetes-1.png)

The components of the platform are therefore:

1. The server prometheus: which collects monitoring data and stores it
2. The grafana part which provides the data visualization part
3. The PushGateway part whose role is to ensure the collection of supervision data which would be collected in push mode (in fact, prometheus supervision mode is the default pull)
4. the Alert manager part which is used to broadcast events for the supervision teams.
5. The export nodes, which constitute the data collection part in pull mode.

It can be noted that for the deployment of this supervision chain in a real production environment it is often necessary to add the following components:

1. [Blackbox](https://geekflare.com/fr/monitor-website-with-blackbox-prometheus-grafana/): for monitoring website performance
2. [Thanos](https://thanos.io/): for advanced long-term storage management

## Tutorial Objectives

In this tutorial we will install the prometheus grafana suite on a Kubernetes cluster in order to monitor its performance. We are going to carry out this work to produce a Chart Helm which will give us the possibility of adding our own Grafana dashboards.

## Prerequisites

You must have access to a Kubernetes cluster on your development computer. For this article we will use K3d, but you can also use [Kind](https://kind.sigs.k8s.io/) or [Minikube](https://kubernetes.io/fr/docs /setup/learning-environment/minikube/).

You also need to have the basic knowledge around creating [Chart Helm](https://helm.sh).

## k3d cluster creation

After installing the k3d tool by following the [installation procedure](https://k3d.io), check the correct installation of the tool by running the following command:

```bash
k3d --version
```

You should get the following result:

```bash
k3d version v3.2.0
k3s version v1.18.9-k3s1 (default)
```

Create your first cluster by running the command

```bash
k3d cluster create mycluster -p "80:80@loadbalancer" --agents 2
```

Note the `-p "80:80@loadbalancer"` parameter which will expose your ingress controller

## Creation of the chart from a model and cleaning of the code

In a terminal, run the following command:

```bash
helm3 create prometheus
```

You should obtain the following tree structure, which corresponds to a basic chart:

```bash
📦prometheus
 ┣ 📂charts
 ┣ 📂templates
 ┃ ┣ 📂tests
 ┃ ┃ ┗ 📜test-connection.yaml
 ┃ ┣ 📜NOTES.txt
 ┃ ┣ 📜_helpers.tpl
 ┃ ┣ 📜deployment.yaml
 ┃ ┣ 📜hpa.yaml
 ┃ ┣ 📜ingress.yaml
 ┃ ┣ 📜service.yaml
 ┃ ┗ 📜serviceaccount.yaml
 ┣ 📜.helmignore
 ┣ 📜Chart.yaml
 ┗ 📜values.yaml
```

Delete unnecessary files, because we don't need deployment, hpa, ingress, service or even service account or even tests. On the other hand we need configmaps and a dashboard folder at the root of the chart. After the cleaning step, our chart looks like this:

```bash
📦prometheus
 ┣ 📂charts
 ┣ 📂dashboards
 ┣ 📂templates
 ┃ ┗ 📂configmap
 ┣ 📜.helmignore
 ┣ 📜Chart.yaml
 ┗ 📜values.yaml
```

## Specialization of the chart

### Editing the Chart.yaml file

Our chart will be based on the one provided by the community: [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) which Underlyingly installs several Kubernetes operators.

In order to configure this dependency, it is therefore necessary to modify the `chart.yaml` file as follows:

```yaml
apiVersion: v2
name: prometheus
description: A Helm chart for Kubernetes

# A chart can be either an 'application' or a 'library' chart.
#
# Application charts are a collection of templates that can be packaged into versioned archives
# to be deployed.
#
# Library charts provide useful utilities or functions for the chart developer. They're included as
# a dependency of application charts to inject those utilities and functions into the rendering
# pipeline. Library charts do not define any templates and therefore cannot be deployed.
type: application

# This is the chart version. This version number should be incremented each time you make changes
# to the chart and its templates, including the app version.
# Versions are expected to follow Semantic Versioning (https://semver.org/)
version: 0.1.0

# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application. Versions are not expected to
# follow Semantic Versioning. They should reflect the version the application is using.
# It is recommended to use it with quotes.
appVersion: "1.16.0"

dependencies:
  - name: kube-prometheus-stack
    version: 14.4.0
    repository: https://prometheus-community.github.io/helm-charts
    import-values:
      - child: grafana
        parent: grafana
```

### Editing the values.yaml file

The values ​​file should be modified as follows:

```yaml
kube-prometheus-stack:
    ## Provide a name in place of kube-prometheus-stack for `app:` labels
    ##
    nameOverride: ""

    ## Override the deployment namespace
    ##
    namespaceOverride: ""

    ## Provide a k8s version to auto dashboard import script example: kubeTargetVersionOverride: 1.16.6
    ##
    kubeTargetVersionOverride: ""

    ## Provide a name to substitute for the full names of resources
    ##
    fullnameOverride: ""

    ## Component scraping the kube controller manager
    kubeControllerManager:
      enabled: false

    ## Component scraping etcd
    kubeEtcd:
      enabled: false

    ## Component scraping kube proxy
    kubeProxy:
      enabled: false

    ## Component scraping kube scheduler
    kubeScheduler:
      enabled: false

    ## Configuration for alertmanager
    ## ref: https://prometheus.io/docs/alerting/alertmanager/
    ##
    alertmanager:
      alertmanagerSpec:
        image:
          repository: quay.io/prometheus/alertmanager
          tag: latest
          sha: ""  
    
    ingress:
    ## If true, Grafana Ingress will be created
    ##
      enabled: true

    ## Annotations for Grafana Ingress
    ##
      annotations: 
      kubernetes.io/ingress.class: nginx
      # kubernetes.io/tls-acme: "true"

    ## Labels to be added to the Ingress
    ##
      labels: {}

    ## Hostnames.
    ## Must be provided if Ingress is enable.
    ##
    # hosts:
    #   - grafana.domain.com
      hosts: 
      - dev-platform

    ## Path for grafana ingress
      path: /grafana/  
    
    ## Manages Prometheus and Alertmanager components
    ##
    prometheusOperator:
      enabled: true
      admissionWebhooks:
        failurePolicy: Fail
        enabled: true
        patch:
          enabled: true
          image:
            repository: jettech/kube-webhook-certgen
            tag: v1.5.1
            sha: ""
            pullPolicy: IfNotPresent
          resources: {}
      ## Prometheus-operator image
      ##
      image:
        repository: quay.io/prometheus-operator/prometheus-operator
        tag: v0.46.0
        sha: ""
        pullPolicy: IfNotPresent

      prometheusConfigReloaderImage:
        repository: quay.io/prometheus-operator/prometheus-config-reloader
        tag: v0.46.0
        sha: ""

    prometheus:
      enabled: true
      ## Configuration for Prometheus service
      ##
      service:
        type: LoadBalancer
      prometheusSpec:
        image:
          repository: quay.io/prometheus/prometheus
          tag: v2.24.0
          sha: ""
        ## External URL at which Prometheus will be reachable.
        ##
        externalUrl: "http://kind-dev/prometheus"
        ## Prefix used to register routes, overriding externalUrl route.
        ## Useful for proxies that rewrite URLs.
        ##
        routePrefix: /prometheus
    grafana:
      adminPassword: "password"
      grafana.ini:
        server:
          domain: cluster-dev.local
          root_url: "%(protocol)s://%(domain)s:%(http_port)s/grafana"
          serve_from_sub_path: true
      image:
        repository: grafana/grafana

      downloadDashboardsImage:
        repository: curlimages/curl

      initChownData:
        image:
          repository: busybox

      sidecar:
        image:
          repository: kiwigrid/k8s-sidecar
      ingress:
        ## If true, Grafana Ingress will be created
        ##
        enabled: true

        ## Annotations for Grafana Ingress
        ##
        #annotations: {
        #  kubernetes.io/ingress.class: nginx,:qa:pod
        #  kubernetes.io/tls-acme: "false"
        #}

        ## Labels to be added to the Ingress
        ##
        labels: {}

        ## Hostnames.
        ## Must be provided if Ingress is enable.
        ##
        # hosts:
        #   - grafana.domain.com
        hosts: 
          - grafana.cluster-dev.local

        ## Path for grafana ingress
        path: /grafana

        ## TLS configuration for grafana Ingress
        ## Secret must be manually created in the namespace
        ##
        tls: []
        # - secretName: grafana-general-tls
        #   hosts:
        #   - grafana.example.com

    kube-state-metrics:
      image:
        repository: quay.io/coreos/kube-state-metrics

    prometheus-node-exporter:
      image:
        repository: quay.io/prometheus/node-exporter
```
Note the `grafana.cluster-dev.local` value that we have set in order to create ingresses to access grafana.

### Modification of the map configuration file in order to add custom grafana dashboards

In the `/template/configmap/` folder of your chart, please add a yaml file to create a custom dashboard configuration configmap. The content of the latter should be as follows:

```yaml
{% raw %}
{{- $files := .Files.Glob "dashboards/*.json" }}
{{- if $files }}
apiVersion: v1
kind: ConfigMapList
items:
{{- range $path, $fileContents := $files }}
{{- $dashboardName := regexReplaceAll "(^.*/)(.*)\\.json$" $path "${2}" }}
- apiVersion: v1
  kind: ConfigMap
  metadata:
    name: {{ printf "%s-%s" (include "prometheus.fullname" $) $dashboardName | trunc 63 | trimSuffix "-" }}
    namespace: {{ $.Release.Namespace }}
    labels:
      {{- if $.Values.grafana.sidecar.dashboards.label }}
      {{ $.Values.grafana.sidecar.dashboards.label }}: "1"
      {{- end }}
      app: {{ template "prometheus.name" $ }}-grafana
{{ include "prometheus.labels" $ | indent 6 }}
  data:
    {{ $dashboardName }}.json: {{ $.Files.Get $path | toJson }}
{{- end }}
{{- end }}
{% endraw %}
```
As you can see, this configuration map is created by going through the list of json files contained in the chart itself. This is why we have set up a dashboards folder in our chart which will contain the json files for defining custom dashboards. You can find an example list at: [https://grafana.com/grafana/dashboards](https://grafana.com/grafana/dashboards)

## Deployment of our chart

### installation of our chart

To install your chart run the following command:

```bash
helm install -n prometheus prometheus ./prometheus/ --create-namespace
```

Launch your favorite Kubernetes management tool to see the list of pods that have been created (here we are using k9s):

![prometheus and grafana pod list](/blog/images/post/2021/03/2021-03-31-deployment-prometheus-grafana-kubernetes-2.png)

## configure your DNS

In order to access your grafana portal you must modify your `/etc/hosts` file in order to add an entry for the following host `grafana.cluster-dev.local`. Indeed, the grafana service is distributed by default through an [ingress controller](https://kubernetes.github.io/ingress-nginx/) and an ingress. Your domain must therefore be known on your host machine.

So add the following entry in your /etc/hosts file

```bash
127.0.0.1 grafana.cluster-dev.local
```

Then simply connect to the following URL [http://grafana.cluster-dev.local/grafana](http://grafana.cluster-dev.local/grafana) to access the connection chart grafana.

![grafana portal](/blog/images/post/2021/03/2021-03-31-deployment-prometheus-grafana-kubernetes-3.png)

Enter the following login `admin` and password `password` to connect to grafana. You will then have access to the list of your dashboards in the manage part.

![grafana dashboard](/blog/images/post/2021/03/2021-03-31-deployment-prometheus-grafana-kubernetes-4.png)

To access the source code of this example, it's here [https://github.com/matthieupetite/helm-samples](https://github.com/matthieupetite/helm-samples)